# Vecka 35: AWS CLI Cheat Sheet

Nedan finner ni nyttiga kommandon för att använda AWS CLI på era datorer.

## Installation

För att installera AWS CLI [klickar du här](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), väljer det operativsystem du sitter på och följer guiden.

`
aws --version
`

Detta kan användas för att kolla vilken version av AWS CLI du har installerat, och används med fördel för att kontrollera att installationen gick bra till.

## Konfiguration

Innan du kan börja koda och pusha din kod till AWS så behöver du koppla upp dig mot ditt AWS-konto. Bege dig till din användarmapp på datorn och välj att visa dolda mappar. Detta gör du genom att leta upp fliken *Visa* och välja *Dolda objekt* på Windows, och genom att hålla in *Cmd + Shift + .* på Mac. Nu bör det dyka upp en mapp som heter *.aws*, och gör det inte det så kan du skapa den (glöm inte punkten!!!). Förhoppningsvis ligger det i mappen 2 filer vid namn *credentials* och *config*, gör det inte det så behöver du skapa dem med hjälp av en terminal då filerna inte skall ha någon filtyp. Hoppa in i *.aws*-mappen och skapa båda filerna enligt följande.

`
New-Item config
`

Detta kommando fungerar i PowerShell på Windows.

`
touch config
`

Detta kommando fungerar på Mac.

När filerna skapats (eller om de fanns där från början) så kan du först klicka dig in på *config* som kommer öppnas i en texteditor och klistra in följande:

`
[default] 
region = eu-north-1
`

Därefter öppnar du *credentials* och lägger in följande:

`
[default]
aws_access_key_id = 
aws_secret_access_key = 
`

Efter likamed-tecknen skriver du in din nyckel och din hemlighet, för det av dina IAM-konton du kommer vilja använda, och sparar. Info om hur du skapar dessa finns i de bifogade filmerna.

## Arbeta lokalt och pusha

Nu när du konfigurerat klart kommer du kunna skriva koden lokalt i VSCode och därefter pusha din lambdafunktion till AWS. Först innan du pushar behöver du dock zippa dina filer då AWS Lambda enbart läser zip.

`
tar -acf index.zip index.mjs
`

Detta används för att zippa din *index.mjs*-fil på Windows.

`
zip index.zip index.mjs
`

Detta används för att zippa din *index.mjs*-fil på Mac.
Notera i båda fallen att du nu skalla stå **Inuti** mappen där filen ligger. Det är filen som skall zippas **Inte** mappen.
Efter att filen zippats så kan vi nu pusha den till AWS genom att köra nedanstående kommando som är detsamma oavsett operativsystem:

`
aws lambda update-function-code --function-name <namnet på din lambda-funktion> --zip-file fileb://index.zip
`

Detta fungerar dock enbart om du redan manuellt skapat din lambdafunktion manuellt på AWS. Att skapa en funktion med hjälp av CLIet är ingenting ni behöver göra, MEN om ni vill testa så är det följande kommando som används för att skapa en lambdafunktion:

`
 aws lambda create-function --function-name <namnet på din lambda-funktion> --runtime nodejs18.x --role <arn-adressen till din roll> --handler index.handler --zip-file fileb://index.zip
`

ARN-adressen är den adress som rollen du vill använda har. Du hittar den om du klickar dig in på en av dina befintliga roller på AWS.

Alla dessa kommandon är inget ni behöver lära er då ni kan lägga in dem som script om ni skapar ett projekt med `npm init -y` så att ni får en *package.json*-fil att leka med.
