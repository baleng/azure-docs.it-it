---
title: Guida introduttiva all'uso di Machine Learning Services (con R) nel database SQL di Azure (anteprima) | Microsoft Docs
description: Questo argomento illustra come usare Machine Learning Services nel database SQL di Azure e come eseguire script R per implementare l'analisi avanzata su larga scala e la possibilità di trasferire i calcoli e l'elaborazione nella posizione in cui risiedono i dati, eliminando la necessità di eseguire il pull dei dati sulla rete.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.reviewer: ''
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: e15cf93514f921223fea37aa480730bba46dd195
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "57864950"
---
# <a name="quickstart-use-machine-learning-services-with-r-in-azure-sql-database-preview"></a>Avvio rapido: Usare Machine Learning Services (con R) nel database SQL di Azure (anteprima)

Questo articolo illustra come usare l'anteprima pubblica di [Machine Learning Services (con R) nel database SQL di Azure](sql-database-machine-learning-services-overview.md). Descrive le nozioni di base dello spostamento di dati tra un database SQL e R e spiega come eseguire il wrapping di codice R ben formato nella stored procedure [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) per compilare, eseguire il training e usare modelli di Machine Learning in un database SQL.

Sfruttare i vantaggi del linguaggio R per implementare l'analisi avanzata e l'apprendimento automatico nel database. Questa possibilità consente di effettuare i calcoli e l'elaborazione dove si trovano i dati, eliminando la necessità di eseguire il pull dei dati attraverso la rete. È anche possibile sfruttare le potenzialità dei pacchetti R aziendali per implementare l'analisi avanzata su larga scala.

Machine Learning Services include una distribuzione di base di R, integrata con i pacchetti R aziendali di Microsoft. Le funzioni e gli algoritmi R di Microsoft sono progettati per essere scalabili e utili e offrire quindi soluzioni di analisi predittiva, modellazione statistica, visualizzazioni dei dati e algoritmi di apprendimento automatico all'avanguardia.

Se non si ha una sottoscrizione di Azure, [creare un account](https://azure.microsoft.com/free/) prima di iniziare.

> [!IMPORTANT]
> Machine Learning Services nel database SQL di Azure è attualmente in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate.
> Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> [Iscriversi per l'anteprima](sql-database-machine-learning-services-overview.md#signup).

## <a name="prerequisites"></a>Prerequisiti

Per eseguire il codice di esempio in questi esercizi, è necessario avere un database SQL con Machine Learning Services (con R) abilitato. Durante l'anteprima pubblica, Microsoft eseguirà l'onboarding e l'abilitazione dell'apprendimento automatico per il database nuovo o esistente. Seguire la procedura descritta in [Iscriversi per l'anteprima](sql-database-machine-learning-services-overview.md#signup).

È possibile connettere al database SQL ed eseguire gli script R su qualsiasi strumento di query o gestione del database, purché possa connettersi a un database SQL ed eseguire una query T-SQL o una stored procedure. In questa guida introduttiva viene usato [SQL Server Management Studio](sql-database-connect-query-ssms.md).

Per l'esercizio [Aggiungere un pacchetto](#add-package), è necessario anche installare [R](https://www.r-project.org/) e [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) nel computer locale.

Questa guida introduttiva richiede anche di configurare una regola del firewall a livello di server. Per consultare la guida introduttiva che illustra come eseguire questa operazione, vedere [Creare una regola di firewall a livello di server](sql-database-server-level-firewall-rule.md).

## <a name="verify-r-exists"></a>Verificare l'esistenza di R

È possibile verificare che Machine Learning Services (con R) sia abilitato per il database SQL. Attenersi alla procedura descritta di seguito.

1. Aprire SQL Server Management Studio e connettersi al database SQL. Per altre informazioni su come connettersi, vedere [Guida introduttiva: Usare SQL Server Management Studio per connettersi a un database SQL di Azure ed eseguire query](sql-database-connect-query-ssms.md).

1. Eseguire il codice riportato di seguito. 

    ```sql
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(31 + 11)';
    GO
    ```
    Se tutto funziona correttamente, verrà visualizzato un messaggio di risultato simile al seguente.

    ```text
    STDOUT message(s) from external script: 
    42
    ```

1. Se si verificano errori, è possibile che l'anteprima pubblica di Machine Learning Services (con R) non sia abilitata per il database SQL in uso. Vedere come iscriversi all'anteprima pubblica nella sezione precedente.

## <a name="grant-permissions"></a>Concedere le autorizzazioni

Gli amministratori possono eseguire il codice esterno automaticamente. A tutti gli altri utenti devono essere concesse le autorizzazioni.

Sostituire `<username>` con un account di accesso utente al database valido prima di eseguire il comando.

```sql
GRANT EXECUTE ANY EXTERNAL SCRIPT TO <username>
```

## <a name="basic-r-interaction"></a>Interazione di base con R

Si può eseguire codice R nel database SQL in due modi:

+ Aggiungere uno script R come argomento della stored procedure di sistema [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).
+ Da un [client R remoto](https://docs.microsoft.com/sql/advanced-analytics/r/set-up-a-data-science-client), connettersi al database SQL ed eseguire codice usando il database SQL come contesto di calcolo.

L'esercizio seguente è incentrato sul primo modello di interazione: come passare codice R a una stored procedure.

1. Eseguire uno script semplice per vedere come viene eseguito uno script R nel database SQL.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R',
    @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c, d)'
    ```

2. Presumendo che tutto sia stato configurato correttamente, viene calcolato il risultato corretto e la funzione `print` di C restituisce il risultato nella finestra **Messaggi**.

    **Risultati**

    ```text
    STDOUT message(s) from external script: 
    0.5 2
    ```

    Anche se ricevere messaggi **stdout** è utile quando si testa il codice, più spesso è necessario restituire i risultati in formato tabulare, in modo da poterli usare in un'applicazione o scrivere in una tabella. Vedere la sezione sugli input e gli output di seguito per altre informazioni.

Tenere presente che tutto il contenuto all'interno dell'argomento `@script` deve essere codice R valido.

## <a name="inputs-and-outputs"></a>Input e output

Per impostazione predefinita [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) accetta un singolo set di dati input, che in genere viene fornito sotto forma di una query SQL valida. Altri tipi di input possono essere passati come variabili SQL.

La stored procedure restituisce come output un singolo frame di dati R, ma è anche possibile restituire valori scalari e modelli come variabili. Ad esempio, è possibile restituire un modello con training come variabile binaria e passarlo a un'istruzione T-SQL INSERT, per scrivere il modello in una tabella. È anche possibile generare tracciati (in formato binario) o valori scalari (singoli valori, ad esempio data e ora, tempo impiegato per eseguire il training del modello e così via).

Per il momento verranno esaminate le variabili di input e output predefinite di sp_execute_external_script: `InputDataSet` e `OutputDataSet`.

1. Creare una piccola tabella di dati di test eseguendo l'istruzione T-SQL seguente:

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    INSERT INTO RTestData VALUES (1);
    INSERT INTO RTestData VALUES (10);
    INSERT INTO RTestData VALUES (100);
    GO
    ```

    Dopo aver creato la tabella, usare l'istruzione seguente per eseguire query sulla tabella:
  
    ```sql
    SELECT * FROM RTestData
    ```

    **Risultati**

    ![Contenuto della tabella RTestData](./media/sql-database-connect-query-r/select-rtestdata.png)

2. È possibile usare i dati dalla tabella come input per lo script R. Eseguire l'istruzione seguente. Ottiene i dati dalla tabella, esegue un round trip attraverso il runtime di R e restituisce i valori con il nome della colonna *NewColName*.

    I dati restituiti dalla query vengono passati al runtime di R, che restituisce i dati al database SQL come frame di dati. La clausola WITH RESULT SETS definisce lo schema della tabella di dati restituita per il database SQL.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    **Risultati**

    ![Output dello script R che restituisce i dati da una tabella](./media/sql-database-connect-query-r/r-output-rtestdata.png)

3. Si modificherà ora il nome delle variabili di input o output. Lo script precedente usa i nomi predefiniti delle variabili di input e output, _InputDataSet_ e _OutputDataSet_. Per definire i dati di input associati a _InputDatSet_, usare la variabile *\@input_data_1*.

    In questo script, i nomi delle variabili di input e output per la stored procedure sono stati modificati in *SQL_out* e *SQL_in*:

    ```sql
    EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N' SQL_out <- SQL_in;'
      , @input_data_1 = N' SELECT 12 as Col;'
      , @input_data_1_name  = N'SQL_in'
      , @output_data_1_name =  N'SQL_out'
      WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    Si noti che R fa distinzione tra maiuscole e minuscole, quindi le variabili di input e output in `@input_data_1_name` e `@output_data_1_name` devono coincidere con quelle nel codice R in `@script`. 

    Anche l'ordine dei parametri è importante. È necessario specificare prima i parametri obbligatori *\@input_data_1* e *\@output_data_1* per poter usare i parametri facoltativi *\@input_data_1_name* e *\@output_data_1_name*.

    È possibile passare come parametro un solo set di dati di input e si può restituire un solo set di dati. Tuttavia, è possibile chiamare altri set di dati dall'interno del codice R e restituire output di altri tipi in aggiunta al set di dati. È anche possibile aggiungere la parola chiave OUTPUT a qualsiasi parametro per fare in modo che venga restituito con i risultati. 

    L'istruzione `WITH RESULT SETS` definisce lo schema per i dati usato nel database SQL. È necessario specificare i tipi di dati compatibili con SQL per ogni colonna restituita da R. Si può usare la definizione dello schema anche per fornire nuovi nomi di colonna, perché non è necessario usare i nomi di colonna dal frame di dati R.

4. È anche possibile generare valori usando lo script R e lasciare la stringa di query di input in _@input_data_1_ vuota.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N' mytextvariable <- c("hello", " ", "world");
            OutputDataSet <- as.data.frame(mytextvariable);'
        , @input_data_1 = N''
    WITH RESULT SETS (([Col1] CHAR(20) NOT NULL));
    ```

    **Risultati**

    ![Risultati della query usando @script come input](./media/sql-database-connect-query-r/r-data-generated-output.png)

## <a name="check-r-version"></a>Verificare la versione di R

Per verificare la versione di R installata nel database SQL, eseguire le operazioni seguenti:

1. Eseguire lo script seguente sul database SQL.

    ```SQL
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(version)';
    GO
    ```

2. La funzione `print` di R restituisce la versione nella finestra **Messaggi**. Nell'output di esempio seguente si può vedere che in questo caso nel database SQL è installata la versione 3.4.4 di R.

    **Risultati**

    ```text
    STDOUT message(s) from external script:
                   _
    platform       x86_64-w64-mingw32
    arch           x86_64
    os             mingw32
    system         x86_64, mingw32
    status
    major          3
    minor          4.4
    year           2018
    month          03
    day            15
    svn rev        74408
    language       R
    version.string R version 3.4.4 (2018-03-15)
    nickname       Someone to Lean On
    ```

## <a name="list-r-packages"></a>Elencare i pacchetti R

Microsoft fornisce una serie di pacchetti R preinstallati con Machine Learning Services nel database SQL. Per visualizzare un elenco dei pacchetti R installati, incluse informazioni su versione, dipendenze, licenza e percorso della libreria, attenersi alla procedura seguente. Per aggiungere altri pacchetti, vedere la sezione [Aggiungere un pacchetto](#add-package).

1. Eseguire lo script seguente sul database SQL.

    ```SQL
    EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
    OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
    WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
        , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
    ```

2. L'output viene da `installed.packages()` in R ed è restituito come set di risultati.

    **Risultati**

    ![Pacchetti installati in R](./media/sql-database-connect-query-r/r-installed-packages.png)

## <a name="create-a-predictive-model"></a>Creare un modello predittivo

È possibile eseguire il training di un modello usando R e salvare il modello in una tabella nel database SQL. In questo esercizio si eseguirà il training di un semplice modello di regressione che stima la distanza di arresto di un'automobile in base alla velocità. Si userà il set di dati `cars` incluso in R, perché è piccolo e facile da comprendere.

1. Prima di tutto, creare una tabella in cui salvare i dati di training.

    ```sql
    CREATE TABLE dbo.CarSpeed (speed INT NOT NULL, distance INT NOT NULL)
    GO
    INSERT INTO dbo.CarSpeed (speed, distance)
    EXEC sp_execute_external_script
    @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

    Molti set di dati di piccole e grandi dimensioni sono inclusi con il runtime di R. Per ottenere un elenco dei set di dati installati con R, digitare `library(help="datasets")` da un prompt dei comandi di R.

2. Creare un modello di regressione. I dati sulla velocità dell'automobile contengono due colonne numeriche, `dist` e`speed`. Sono presenti più osservazioni di alcune velocità. Da questi dati, si creerà un modello di regressione lineare che descrive una relazione tra la velocità dell'automobile e la distanza necessaria per l'arresto.

    I requisiti di un modello lineare sono semplici:

   - Definire una formula che descrive la relazione tra la variabile dipendente `speed` e la variabile indipendente `distance`.

   - Fornire i dati di input da usare per eseguire il training del modello.

     > [!TIP]
     > Se occorre un ripasso sui modelli lineari, questa esercitazione descrive il processo di adattamento di un modello tramite rxLinMod: [Fitting Linear Models](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model) (Adattamento di modelli lineari)

     Per compilare il modello, definire la formula all'interno del codice R e passare i dati come parametro di input.

     ```sql
     DROP PROCEDURE IF EXISTS generate_linear_model;
     GO
     CREATE PROCEDURE generate_linear_model
     AS
     BEGIN
       EXEC sp_execute_external_script
       @language = N'R'
       , @script = N'lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
           trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));'
       , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
       , @input_data_1_name = N'CarsData'
       , @output_data_1_name = N'trained_model'
       WITH RESULT SETS ((model VARBINARY(max)));
     END;
     GO
     ```

     Il primo argomento per rxLinMod è il parametro *formula*, che definisce la distanza come dipendente dalla velocità. I dati di input vengono archiviati nella variabile `CarsData`, che viene popolata dalla query SQL. Se non si assegna un nome specifico ai dati di input, il nome predefinito della variabile è _InputDataSet_.

2. Successivamente, creare una tabella in cui archiviare il modello in modo che sia possibile ripetere il training o usarlo per la stima. L'output di un pacchetto R che crea un modello è in genere un **oggetto binario**. Di conseguenza, la tabella deve contenere una colonna di tipo **VARBINARY(max)**.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
    );
    ```
3. Per salvare il modello, eseguire l'istruzione Transact-SQL seguente per chiamare la stored procedure, generare il modello e salvarlo in una tabella.

    ```sql
    INSERT INTO dbo.stopping_distance_models (model)
    EXEC generate_linear_model;
    ```

    Si noti che, se si esegue questo codice una seconda volta, viene visualizzato questo errore:

    ```text
    Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object dbo.stopping_distance_models
    ```

    Un modo per evitare questo errore consiste nell'aggiornare il nome per ogni nuovo modello. Ad esempio, si può modificare il nome scegliendone uno più descrittivo e includere il tipo di modello, il giorno di creazione e così via.

    ```sql
    UPDATE dbo.stopping_distance_models
    SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
    WHERE model_name = 'default model'
    ```

4. In generale, l'output di R dalla stored procedure [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) è limitato a un singolo frame di dati.

    Tuttavia, oltre al frame di dati è possibile restituire output di altri tipi, ad esempio valori scalari.

    Si supponga ad esempio di voler eseguire il training di un modello, ma visualizzare immediatamente una tabella di coefficienti dal modello. È possibile creare la tabella di coefficienti come set di risultati principale e restituire il modello con training in una variabile SQL. È possibile riutilizzare immediatamente il modello chiamando la variabile oppure salvarlo in una tabella come illustrato di seguito.

    ```sql
    DECLARE @model VARBINARY(max), @modelname VARCHAR(30)
    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
            speedmodel <- rxLinMod(distance ~ speed, CarsData)
            modelbin <- serialize(speedmodel, NULL)
            OutputDataSet <- data.frame(coefficients(speedmodel));'
        , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
        , @input_data_1_name = N'CarsData'
        , @params = N'@modelbin varbinary(max) OUTPUT'
        , @modelbin = @model OUTPUT
        WITH RESULT SETS (([Coefficient] FLOAT NOT NULL));

    -- Save the generated model
    INSERT INTO dbo.stopping_distance_models(model_name, model)
    VALUES ('latest model', @model)
    ```

    **Risultati**

    ![Modello con training con output aggiuntivo](./media/sql-database-connect-query-r/r-train-model-with-additional-output.png)

## <a name="predict"></a>Eseguire stime

Usare il modello creato nella sezione precedente per assegnare un punteggio alle stime in base a nuovi dati. Per eseguire l'_assegnazione dei punteggi_ usando nuovi dati, ottenere uno dei modelli con training dalla tabella e quindi chiamare un nuovo set di dati su cui basare le stime. Assegnazione dei punteggi è un termine usato talvolta nella data science per indicare la generazione di stime, probabilità o altri valori in base a nuovi dati inseriti in un modello con training.

1. In primo luogo, creare una tabella con i nuovi dati di velocità. Si noti che i dati di training originali terminano in corrispondenza di una velocità pari a 25 miglia orarie. Infatti, i dati originali sono basati su un esperimento del 1920. Ci si potrebbe chiedere quanto tempo è necessario a un'automobile del 1920 per arrestarsi, presupponendo che possa arrivare a 60 o persino 100 miglia l'ora. Per rispondere a questa domanda è necessario fornire alcuni nuovi valori di velocità.

    ```sql
    CREATE TABLE dbo.NewCarSpeed(speed INT NOT NULL,
        distance INT NULL
    )
    GO
    INSERT dbo.NewCarSpeed(speed)
    VALUES (40), (50), (60), (70), (80), (90), (100)
    ```

    In questo esempio, poiché il modello è basato sull'algoritmo **rxLinMod** fornito come parte del pacchetto **RevoScaleR**, si chiama la funzione [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) anziché la funzione generica `predict` di R.

    ```sql
    DECLARE @speedmodel varbinary(max) = 
        (SELECT model FROM dbo.stopping_distance_models WHERE model_name = 'latest model');

    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
                current_model <- unserialize(as.raw(speedmodel));
                new <- data.frame(NewCarData);
                predicted.distance <- rxPredict(current_model, new);
                str(predicted.distance);
                OutputDataSet <- cbind(new, ceiling(predicted.distance));
                '
        , @input_data_1 = N'SELECT speed FROM [dbo].[NewCarSpeed]'
        , @input_data_1_name = N'NewCarData'
        , @params = N'@speedmodel varbinary(max)'
        , @speedmodel = @speedmodel
    WITH RESULT SETS ((new_speed INT, predicted_distance INT));
    ```

    Lo script precedente esegue queste operazioni:

   + Usa un'istruzione SELECT per ottenere un singolo modello dalla tabella e passarlo come parametro di input.

   + Dopo aver recuperato il modello dalla tabella, chiama la funzione `unserialize` sul modello.

       > [!TIP] 
       > Vedere anche le nuove [funzioni di serializzazione](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) fornite da RevoScaleR, che supportano l'assegnazione di punteggi in tempo reale.
   + Applica la funzione `rxPredict` con gli argomenti appropriati al modello e fornisce i nuovi dati di input.

   + Nell'esempio, la funzione `str` viene aggiunta durante la fase di test per verificare lo schema dei dati restituiti da R. È possibile rimuovere l'istruzione in un secondo momento.

   + I nomi di colonna usati nello script R non vengono necessariamente passati all'output della stored procedure. Qui è stata usata la clausola WITH RESULTS per definire alcuni nuovi nomi di colonna.

     **Risultati**

     ![Set di risultati per la stima della distanza di arresto](./media/sql-database-connect-query-r/r-predict-stopping-distance-resultset.png)

     È anche possibile usare [PREDICT in Transact-SQL](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) per generare un valore stimato o un punteggio in base a un modello archiviato.

<a name="add-package"></a>

## <a name="add-a-package"></a>Aggiungere un pacchetto

Se è necessario usare un pacchetto che non è già installato nel database SQL, è possibile installarlo tramite [sqlmlutils](https://github.com/Microsoft/sqlmlutils). Seguire questa procedura per installare il pacchetto.

1. Scaricare nel computer locale la versione più recente del file ZIP **sqlmlutils** da [github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist). Non è necessario decomprimere il file.

1. Se R non è installato, scaricarlo da [www.r-project.org](https://www.r-project.org/) e installarlo nel computer locale. R è disponibile per Windows, MacOS e Linux. In questo esempio viene usato Windows.

1. Prima di tutto installare il pacchetto **RODBCext**, che è un prerequisito per **sqlmlutils**. **RODBCext** installa anche il pacchetto di dipendenza **RODBC**. Aprire un **prompt dei comandi** ed eseguire il comando seguente:

    ```
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    ```

    Se si riceve un errore simile a "'R' non è riconosciuto come comando interno o esterno, un programma eseguibile o un file batch", probabilmente il percorso di R.exe non è incluso nella variabile di ambiente **PATH** in Windows. È possibile aggiungere la directory alla variabile di ambiente o passare alla directory nel prompt dei comandi (ad esempio `cd C:\Program Files\R\R-3.5.1\bin`) prima di eseguire il comando.

1. Usare il comando **R CMD INSTALL** per installare **sqlmlutils**. Specificare il percorso della directory in cui è stato scaricato il file ZIP e il nome del file ZIP. Ad esempio: 

    ```
    R CMD INSTALL C:\Users\youruser\Downloads\sqlmlutils_0.5.0.zip
    ```

    L'output dovrebbe essere simile al seguente:

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/youruser/Documents/R/win-library/3.5'
    package 'sqlmlutils' successfully unpacked and MD5 sums checked
    ```

1. In questo esempio si userà come ambiente di sviluppo integrato RStudio Desktop. Se si preferisce, è possibile usare un altro IDE. Se RStudio non è già installato, scaricarlo e installarlo da [www.rstudio.com/products/rstudio/download/](https://www.rstudio.com/products/rstudio/download/).

1. Aprire **RStudio** e creare un nuovo file **Script R**. Usare il codice R seguente per installare un pacchetto con sqlmlutils. Nell'esempio seguente si installerà il pacchetto [glue](https://cran.r-project.org/web/packages/glue/), che può formattare e interpolare una stringa.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!NOTE]
    > L'ambito (**scope**) può essere **PUBLIC** o **PRIVATE**. L'ambito pubblico è utile all'amministratore del database per installare pacchetti che tutti gli utenti possono usare. L'ambito privato rende il pacchetto disponibile solo per l'utente che ne esegue l'installazione. Se non si specifica l'ambito, il valore predefinito è **PRIVATE**.

1. Verificare ora che il pacchetto **glue** sia stato installato.

    ```R
    r<-sql_installed.packages(connectionString = connection, fields=c("Package", "LibPath", "Attributes", "Scope"))
    View(r)
    ```

    **Risultati**

    ![Contenuto della tabella RTestData](./media/sql-database-connect-query-r/r-verify-package-install.png)


1. Dopo aver installato il pacchetto, è possibile usarlo nello script R tramite **sp_execute_external_script**. Aprire **SQL Server Management Studio** e connettersi al database SQL. Eseguire lo script seguente:

    ```sql
    EXEC sp_execute_external_script @language = N'R'
    , @script = N'
    library(glue)

    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
        ''my age next year is {age + 1}, '',
        ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    print(text)
    ';
    ```

    Nella scheda Messaggi verrà visualizzato il risultato seguente.

    **Risultati**

    ```text
    STDOUT message(s) from external script:
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

1. Se si vuole eliminare il pacchetto, eseguire lo script R seguente in **RStudio** nel computer locale.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC") 
    ```

> [!NOTE]
> Un altro modo per installare pacchetti R nel database SQL consiste nel caricare il pacchetto dal flusso di byte con [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su Machine Learning Services, vedere gli articoli seguenti. Anche se alcuni di questi articoli sono per SQL Server, la maggior parte delle informazioni è applicabile anche a Machine Learning Services (con R) nel database SQL di Azure.

- [Machine Learning Services (con R) nel database SQL di Azure](sql-database-machine-learning-services-overview.md)
- [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Esercitazione: Informazioni sull'analisi nel database con R in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)
- [End-to-end data science walkthrough for R and SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) (Procedura dettagliata di data science end-to-end per R e SQL Server)
- [Esercitazione: Usare funzioni R di RevoScaleR con dati di SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages)
