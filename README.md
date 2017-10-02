# utl_merge_tables_with_same_colnames
Allow users to add a suffix for column names in one of the tables

    ```  Merge two SAS tables with the same variable names  ```
    ```    ```
    ```  I still do not completely understand 'common storage' and  ```
    ```  multiple macro symbol tables, We need many examples from SAS.  ```
    ```  However this seems to work.  ```
    ```    ```
    ```  WORKING CODE  ```
    ```  ============  ```
    ```    ```
    ```    %varSfx(sashelp.class,sfx=_new);  ```
    ```    merge sashelp.class sashelp.class(rename=(&names));  ```
    ```    ```
    ```  HAVE  ```
    ```  ====  ```
    ```    ```
    ```    SASHELP.CLASS total obs=19  ```
    ```    ```
    ```    Obs    NAME       SEX    AGE    HEIGHT    WEIGHT  ```
    ```    ```
    ```      1    Alfred      M      14     69.0      112.5  ```
    ```      2    Alice       F      13     56.5       84.0  ```
    ```      3    Barbara     F      13     65.3       98.0  ```
    ```      4    Carol       F      14     62.8      102.5  ```
    ```      5    Henry       M      14     63.5      102.5  ```
    ```  ...  ```
    ```    ```
    ```  WANT  ```
    ```  ====  ```
    ```    ```
    ```  WORK.WANT total obs=19  ```
    ```    ```
    ```   NAME     SEX  AGE  HEIGHT  WEIGHT    NAME_NEW SEX_NEW AGE_NEW HEIGHT_NEW WEIGHT_NEW  ```
    ```    ```
    ```   Alfred    M    14   69.0    112.5    Alfred     M       14       69.0      112.5  ```
    ```   Alice     F    13   56.5     84.0    Alice      F       13       56.5       84.0  ```
    ```   Barbara   F    13   65.3     98.0    Barbara    F       13       65.3       98.0  ```
    ```   Carol     F    14   62.8    102.5    Carol      F       14       62.8      102.5  ```
    ```   Henry     M    14   63.5    102.5    Henry      M       14       63.5      102.5  ```
    ```    ```
    ```    ```
    ```  *                _               _       _  ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _  ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |  ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |  ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```   Just use sashelp.class;  ```
    ```    ```
    ```    ```
    ```  *          _       _   _  ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __  ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \  ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |  ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```    ```
    ```  proc datasets lib=work;  ```
    ```  delete twotbl;  ```
    ```  run;quit;  ```
    ```    ```
    ```  %symdel ds sfx names / nowarn;  ```
    ```    ```
    ```  %macro varSfx(ds,Sfx=);  ```
    ```     %symdel ds sfx names / nowarn;  ```
    ```     if _n_=0 then do;  ```
    ```      %let rc=%sysfunc(dosubl('  ```
    ```         %symdel ds sfx names / nowarn;  ```
    ```         proc transpose data=&ds(obs=1) out=__xpo(keep=_name_);  ```
    ```         var _all_;  ```
    ```         run;quit;  ```
    ```         proc sql;  ```
    ```            select cats(_name_,"=",_name_,"&sfx") into :names separated by " "  ```
    ```            from __xpo;  ```
    ```         ;quit;  ```
    ```      '));  ```
    ```      end;  ```
    ```  %mend varSfx;  ```
    ```    ```
    ```  %symdel ds sfx names; * not a bad idea because there can be two ds and sfx;  ```
    ```  data want;  ```
    ```    %varSfx(sashelp.class,sfx=_new);  ```
    ```    merge sashelp.class sashelp.class(rename=(&names));  ```
    ```  run;quit;  ```
    ```    ```
