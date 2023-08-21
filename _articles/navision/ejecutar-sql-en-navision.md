---
title: ""
summary: ""
layout: article
#cSpell:disable
category: [SQL, Navision]
custom_type: Boveda
permalink: /boveda/
#cSpell:enable
author: Esteve Sanpons
date: 2023-08-21 02:00:00 +0200
LinkedIn: false
posible_blog: true
---

Rebuscando por mi baúl de los recursos he encontrado una funcionalidad que hice hace mucho y que en su momento le saque mucho jugo.
Es una manera que des de NAV podamos ejecutar sentencias de SQL, imaginar las posibilidades que tiene esto.

Rescatar datos complejos y hacer cargas masivas, visualizar datos de una BBDD que no sea de Navision, o que este en otro servidor dentro de nuestra red, siempre desde nuestro magnifico Navision

Lo primero es crear las variables globales

```javascript
VAR
      g_SQLConnection@1000000000 : DotNet "'System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.System.Data.SqlClient.SqlConnection";
      g_ServerName@1000000002 : Text;
      g_DBName@1000000001 : Text;
      g_EvitarFormateo@1000000003 : Boolean;
      SqlUpdateLabel@1000000004 : TextConst 'ENU="UPDATE [dbo].[CRONUS España S_A_$Customer] SET [Name] = ''Esanpons'' WHERE [No_] = ''01121212''";ESP="UPDATE [dbo].[CRONUS España S_A_$Customer] SET [Name] = ''Esanpons'' WHERE [No_] = ''01121212''"';
      SqlSelectLabel@1000000005 : TextConst 'ENU=SELECT * FROM [CRONUS España S_A_$Customer];ESP=SELECT * FROM [CRONUS España S_A_$Customer]';


```

<br><br>

La primera es la dotnet que vamos a usar en todo el desarrollo
Las otras 2 son datos que vamos a requerir como el nombre del servidor y la base de datos

La primera función que crearemos es la de inicializar, que requeriremos en todas nuestras conexiones.

```javascript
  PROCEDURE InitSQLConnection@1000000009(ServerName@1000000000 : Text;DBName@1000000001 : Text);
    VAR
      txt01@1000000002 : TextConst 'ENU="Data Source=%1;Initial Catalog=%2;Integrated Security=SSPI";ESP="Data Source=%1;Initial Catalog=%2;Integrated Security=SSPI"';
    BEGIN
      //iniciar la conexión
      CLEAR(g_SQLConnection);

      g_ServerName := ServerName;
      g_DBName := DBName;

      g_SQLConnection := g_SQLConnection.SqlConnection(STRSUBSTNO(txt01,ServerName,DBName));
      g_SQLConnection.Open;
    END;

```

<br><br>

Rellenamos las variables globales e inicializamos la conexión con el SQL.

Después como no la función de cerrar la conexión

```javascript
 PROCEDURE CloseSQLConnection@1000000011();
    BEGIN
      g_SQLConnection.Close;
      CLEAR(g_SQLConnection);
    END;

```

<br><br>

Vamos a la primera de las dos funciones para ejecutar las querys.

Lo primero son las variables que vamos a necesitar

La variable SQLCommand es para poder ejecutar nuestro sentencia SQL

```javascript
 PROCEDURE ExecuteQuerySQL@1000000018(CommandString@1000 : Text) : Integer;
    VAR
      SQLCommand@1003 : DotNet "'System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.System.Data.SqlClient.SqlCommand";
      NoOfAffectedRows@1001 : Integer;
    BEGIN
      //ejecutar querys de modificacion
      CLEAR(SQLCommand);

      SQLCommand := SQLCommand.SqlCommand(CommandString,g_SQLConnection);
      SQLCommand.CommandTimeout(0);
      NoOfAffectedRows := SQLCommand.ExecuteNonQuery;

      EXIT(NoOfAffectedRows);
    END;

```

<br><br>

Como podéis ver lo que haremos es añadir la sentencia SQL y la conexión SQL a nuestro comando de SQL y haremos que nos devuelva las filas afectadas si las hubiera.

Esta función la suelo utilizar para update o delete de datos.

Vale ahora vamos a ir a la compleja la que hace un select y lo añade a una tabla temporal para poder recorrerla posteriormente y tener los datos directamente des del SQL.

Se compone de tres funciones y vamos a ir des de la de mas abajo hasta la que usaremos para rellenar todos los datos. Las primeras son de formateo de datos de inserción o de conversión de datos.

```javascript
LOCAL PROCEDURE InsertValue@1000000000(VAR Value@1000000002 : Variant;SqlDataReader@1000000001 : DotNet "'System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.System.Data.SqlClient.SqlDataReader";NameFieldRef@1000000000 : Text) : Boolean;
    BEGIN
      Value := SqlDataReader.Item(NameFieldRef);
      EXIT(TRUE);
    END;

```

<br><br>

En esta función lo que haremos es añadir en la variable Value el valor rescatado de nuestra conexión de SQL. Le pasamos el nombre del campo y por referencia nos devolverá ese valor.

Ahora para poder entender las siguientes funciones tengo que poner las dos juntas

```javascript
LOCAL PROCEDURE StringConvert@2(NAVName@1000 : Text[80]) : Text[80];
    VAR
      dbpropertyInitialized@1000000002 : Boolean;
      dbpropFromStr@1000000001 : Text[30];
      dbpropToStr@1000000000 : Text[30];
      i@1001 : Integer;
    BEGIN
      IF NOT dbpropertyInitialized THEN BEGIN
        dbpropertyInitialized := TRUE;
        IF GetDBPropertyField('convertidentifiers') = '1' THEN BEGIN
          dbpropFromStr := GetDBPropertyField('invalididentifierchars');
          FOR i := 1 TO STRLEN(dbpropFromStr) DO
            dbpropToStr += '_';
        END;
      END;

      EXIT(CONVERTSTR(NAVName,dbpropFromStr,dbpropToStr));
    END;

    LOCAL PROCEDURE GetDBPropertyField@11(FieldName@1000 : Text) : Text;
    VAR
      SQLCommand@1003 : DotNet "'System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.System.Data.SqlClient.SqlCommand";
      SQLConnection@1002 : DotNet "'System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.System.Data.SqlClient.SqlConnection";
      QuerySQL@1004 : Text;
      FieldValue@1005 : Text;
      l_SQLMgtUtils@1000000000 : Codeunit 50077;
    BEGIN
      IF FieldName = '' THEN
        EXIT('');

      QuerySQL := STRSUBSTNO('select top 1 [%1] from [$ndo$dbproperty]',FieldName);

      CLEAR(l_SQLMgtUtils);

      l_SQLMgtUtils.InitSQLConnection(g_ServerName,g_DBName);
      l_SQLMgtUtils.GetSQLConnection(SQLConnection);

      SQLCommand := SQLCommand.SqlCommand(QuerySQL,SQLConnection);
      SQLCommand.CommandTimeout(0);

      FieldValue := FORMAT(SQLCommand.ExecuteScalar);

      SQLConnection.Close;
      l_SQLMgtUtils.CloseSQLConnection();

      EXIT(FieldValue);
    END;


```

<br><br>

La segunda lo que hace es hacer llamadas al SQl para encontrar datos que requeriremos en la primera función.

La primera función lo que hace es básicamente mirar si tenemos que hacer conversión de datos y recoger cuales son los valores a convertir y convertirlos si hace falta para no tener errores en el SQL ni en nuestras sentencias porque los campos se llaman diferente en nuestro Navision y en el SQL.

Ahora por fin vamos a la función para convertir los datos en datos de una rec de Navision.

Lo primero es inicilizar nuestras variables y ejecutar el comando para que nos retorne todo en un Reader que lo iremos recorriendo.

La sentencia ‘SQL SELECT TOP 1 [CONVERTIDENTIFIERS] FROM [$NDO$DBPROPERTY]’ es utilizada para recuperar el valor de la propiedad CONVERTIDENTIFIERS de la tabla de propiedades de la base de datos en NDO (Network Data Objects). Esta tabla almacena información sobre las propiedades y configuraciones de una base de datos NDO, y el valor de la propiedad CONVERTIDENTIFIERS determina si los identificadores en las consultas SELECT deben ser convertidos a mayúsculas o minúsculas antes de ser enviados al motor de base de datos. Al recuperar solo el primer registro (TOP 1) se asegura que se devuelva solo un resultado

Ahora recorremos el reader y por cada registro que ha devuelto añadiremos ere mismo numero de registro en nuestra tabla. Recorriendo todos los campos de la tabla i añadiendo los datos que necesitamos, pero como el dato nos vendrá en formatos diferentes a veces tendremos que formatearlos para que los podamos añadir en nuestra tabla temporal

Después como he dicho tendremos que validar cada tipo de campo por separado.

```javascript
PROCEDURE GetSQLSelectToRec@1000000029(QuerySQL@1000000003 : Text;VAR RecRef@1000000011 : RecordRef);
    VAR
      SqlCommand@1000000001 : DotNet "'System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.System.Data.SqlClient.SqlCommand";
      SqlDataReader@1000000002 : DotNet "'System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.System.Data.SqlClient.SqlDataReader";
      txt_Type@1000000005 : Text;
      Value@1000000007 : Variant;
      Count@1000000008 : Integer;
      Index@1000000009 : Integer;
      NameFieldRef@1000000013 : Text;
      Field_Ref@1000000012 : FieldRef;
      Value_Time@1000000017 : Time;
      Value_Date@1000000004 : Date;
      DateText@1000000010 : Text;
      TimeText@1000000016 : Text;
      Posicion@1000000015 : Integer;
      error01@1000000000 : TextConst 'ENU=Type regardless: %1;ESP=Tipo sin tener en cuenta: %1';
      txt_DateTimeInit@1000000006 : TextConst 'ENU=01/01/1753 00:00;ESP=01/01/1753 00:00';
      TempBlob@1000000014 : Record 99008535;
      txtClass@1000000018 : Text;
      txt_ClassNormal@1000000019 : TextConst 'ENU=Normal;ESP=Normal';
    BEGIN
      //funcion para guardar en cualquier tabla temporal los datos del SQL
      CLEAR(SqlCommand);
      CLEAR(SqlDataReader);

      //C�digo para ejecutar un comando:
      SqlCommand := g_SQLConnection.CreateCommand();
      SqlCommand.CommandText := QuerySQL;
      SqlDataReader := SqlCommand.ExecuteReader();

      //Codigo para leer un DataReader:
      WHILE SqlDataReader.Read() DO BEGIN
        //Count := SqlDataReader.FieldCount;
        Count := RecRef.FIELDCOUNT;
        FOR Index :=1 TO Count DO BEGIN

          CLEAR(Value);

          Field_Ref := RecRef.FIELDINDEX(Index);
          txt_Type := FORMAT(Field_Ref.TYPE);
          NameFieldRef := FORMAT(Field_Ref.NAME);

          txtClass := FORMAT(Field_Ref.CLASS);
          IF  txtClass = txt_ClassNormal THEN BEGIN

            IF NOT g_EvitarFormateo THEN
              NameFieldRef := CONVERTSTR(StringConvert(NameFieldRef),'.','_');

            IF InsertValue(Value,SqlDataReader,NameFieldRef) THEN BEGIN
              CASE txt_Type OF
                'Code','Text','OemText':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                'Integer':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                'Boolean':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                'Decimal':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                'BigInteger':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                'Date':
                  BEGIN
                    IF FORMAT(Value) <> txt_DateTimeInit THEN BEGIN
                      DateText := FORMAT(Value);
                      Posicion := STRPOS(DateText,' ')-1;

                      DateText := COPYSTR(DateText,1,Posicion);
                      EVALUATE(Value_Date,DateText);

                      Field_Ref.VALUE(Value_Date);
                    END;
                  END;
                'DateFormula':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                'DateTime':
                  BEGIN
                    IF FORMAT(Value) <> txt_DateTimeInit THEN BEGIN
                      Field_Ref.VALUE(Value);
                    END;
                  END;
                'Option':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                'Time':
                  BEGIN
                    IF FORMAT(Value) <> txt_DateTimeInit THEN BEGIN
                      TimeText := FORMAT(Value);
                      Posicion := STRPOS(TimeText,' ')+1;

                      TimeText := COPYSTR(TimeText,Posicion,STRLEN(TimeText));
                      EVALUATE(Value_Time,TimeText);
                      Field_Ref.VALUE(Value_Time);
                    END;
                  END;
                'BLOB':
                  BEGIN
                    //Este lo dejamos vacio para que lo salte
                  END;
                'Media':
                  BEGIN
                    //Este lo dejamos vacio para que lo salte
                  END;
                'GUID':
                  BEGIN
                    Field_Ref.VALUE(Value);
                  END;
                ELSE
                  BEGIN
                    ERROR(error01,txt_Type);
                  END;
              END;
            END;
          END;
        END;

        //Insertar despues de cada linea
        RecRef.INSERT;
      END;
    END;

```

<br><br>

Al final daremos error si no estuviera el tipo de campo contemplado y por ultimo insertaremos.

Al final para poder ejecutar todo el proceso solo requeriremos unas pocas líneas como os muestro a continuación

```javascript
//ejecutar una sentencia SQL devolvera el numero de filas afectadas
InitSQLConnection(NombreServidor, NombreBBDD);
ExecuteQuerySQL(SqlUpdateLabel);
CloseSQLConnection();

//hace un select y devuelve en un rec temporal los datos.
RecRef.OPEN(18, true, COMPANYNAME);
InitSQLConnection(NombreServidorTest, NombreBBDDTest);
GetSQLSelectToRec(SqlSelectLabel, RecRef);
CloseSQLConnection();
```

<br><br>

Como siempre este ejemplo entero lo tenéis colgado en [GitHub](https://github.com/Esanpons/ejemplos-blog/tree/main/CAL/MgtSQL)
