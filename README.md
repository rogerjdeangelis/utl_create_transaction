# utl_create_transaction
Give old and updated table create the transaction table that will recreate the opdated table from the old table

    ``` T003300 After updating a dataset create a transaction dataset that describes all the changes             ```
    ```                                                                                                          ```
    ```    You have done an update and now you want to build a dataset that describes the changes                ```
    ```                                                                                                          ```
    ```     WORKING CODE  Dataset repdelta describes the changes going from dataset olrep to newrep.             ```
    ```       SAS                                                                                                ```
    ```                                                                                                          ```
    ```        %utl_delta                                                                                        ```
    ```         (                                                                                                ```
    ```          uinmem1 =oldrep,        /* Last Months Data */                                                  ```
    ```          uinmem2 =newrep,        /* Current Month Data */                                                ```
    ```          uinkey  =rep_socs,      /* primary unique key both tables */                                    ```
    ```          uotmem1 =repdelta,      /* delta tble for RDBMS update */                                       ```
    ```          uotmem2= repsame                                                                                ```
    ```         );                                                                                               ```
    ```                                                                                                          ```
    ```     Note repdelta can be used for a faster update of oldrep, if you lose the updated dataset             ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ``` see                                                                                                      ```
    ``` https://goo.gl/8GPrjF                                                                                    ```
    ``` https://communities.sas.com/t5/Base-SAS-Programming/Merging-two-dataset-with-a-key/m-p/376653            ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ``` HAVE Datasets before and after update and you want to know what changed                                  ```
    ``` ======================================================================                                   ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ``` Up to 40 obs WORK.OLDREP total obs=7                                                                     ```
    ```                                                                                                          ```
    ``` Obs    REP_SOCS     JOB             ACTIVE                                                               ```
    ```                                                                                                          ```
    ```  1     001001110    carpenter1       YES                                                                 ```
    ```  2     002001110    plumber1         YES                                                                 ```
    ```  3     003001110    mason1           YES                                                                 ```
    ```  4     004001110    plumber1         YES                                                                 ```
    ```  5     005001110    electrician1     YES                                                                 ```
    ```  6     006001110    mason3           YES                                                                 ```
    ```  7     008001110    mason4           NO                                                                  ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ``` Up to 40 obs WORK.NEWREP total obs=6                                                                     ```
    ```                                                                                                          ```
    ``` Obs    REP_SOCS     JOB             ACTIVE                                                               ```
    ```                                                                                                          ```
    ```  1     001001110    carpenter1       YES                                                                 ```
    ```  2     002001110    plumber1         YES                                                                 ```
    ```  3     003001110    painter1         YES                                                                 ```
    ```  4     004001110    plumber1         YES                                                                 ```
    ```  5     005001110    electrician1     YES                                                                 ```
    ```  6     007001110    painter2         YES                                                                 ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ``` WANT                                                                                                     ```
    ``` ====                                                                                                     ```
    ```                                                                                                          ```
    ``` Up to 40 obs from repdelta total obs=4                                                                   ```
    ```                                                                                                          ```
    ```                                            RECORD    TYPE OF                                             ```
    ``` Obs    REP_SOCS       JOB       ACTIVE     SOURCE    CHANGE                                              ```
    ```                                                                                                          ```
    ```  1     006001110    mason3       YES        OLD      DELETE  (record deleted  )                          ```
    ```  2     008001110    mason4       NO         OLD      DELETE  (record deleted  )                          ```
    ```  3     007001110    painter2     YES        NEW      INSERT  (record inserted )                          ```
    ```  4     003001110    painter1     YES        NEW      UPDATE  ('mason1' changed to 'painter1')            ```
    ```                                                                                                          ```
    ``` Note you can use this repdelta on old to recreate nerep                                                  ```
    ```                                                                                                          ```
    ``` *                _              _       _                                                                ```
    ```  _ __ ___   __ _| | _____    __| | __ _| |_ __ _                                                         ```
    ``` | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |                                                        ```
    ``` | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |                                                        ```
    ``` |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|                                                        ```
    ```                                                                                                          ```
    ``` ;                                                                                                        ```
    ```                                                                                                          ```
    ``` data oldrep;  /* rep_socs is primary ket */                                                              ```
    ```   input rep_socs : $9. JOB  : $16. ACTIVE  : $3.;                                                        ```
    ``` cards;                                                                                                   ```
    ```    001001110  carpenter1       YES                                                                       ```
    ```    002001110  plumber1         YES                                                                       ```
    ```    003001110  mason1           YES                                                                       ```
    ```    004001110  plumber1         YES                                                                       ```
    ```    005001110  electrician1     YES                                                                       ```
    ```    006001110  mason3           YES                                                                       ```
    ```    008001110  mason4           NO                                                                        ```
    ``` ;                                                                                                        ```
    ``` run;                                                                                                     ```
    ```                                                                                                          ```
    ``` data newrep;                                                                                             ```
    ```   input rep_socs : $9. JOB  : $16. ACTIVE  : $3.;                                                        ```
    ``` cards;                                                                                                   ```
    ```    001001110  carpenter1       YES                                                                       ```
    ```    002001110  plumber1         YES                                                                       ```
    ```    003001110  painter1         YES                                                                       ```
    ```    004001110  plumber1         YES                                                                       ```
    ```    005001110  electrician1     YES                                                                       ```
    ```    007001110  painter2         YES                                                                       ```
    ``` ;                                                                                                        ```
    ``` run;                                                                                                     ```
    ``` *          _       _   _                                                                                 ```
    ```  ___  ___ | |_   _| |_(_) ___  _ __                                                                      ```
    ``` / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                     ```
    ``` \__ \ (_) | | |_| | |_| | (_) | | | |                                                                    ```
    ``` |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                    ```
    ```                                                                                                          ```
    ``` ;                                                                                                        ```
    ```                                                                                                          ```
    ``` %utl_delta                                                                                               ```
    ```     (                                                                                                    ```
    ```      uinmem1 =oldrep,        /* Last Months Data */                                                      ```
    ```      uinmem2 =newrep,        /* Current Month Data */                                                    ```
    ```      uinkey  =rep_socs,      /* primary unique key both tables */                                        ```
    ```      uotmem1 =repdelta,      /* delta tble for RDBMS update */                                           ```
    ```      uotmem2= repsame                                                                                    ```
    ```     );                                                                                                   ```
    ```                                                                                                          ```
    ``` %utl_update                                                                                              ```
    ```    (                                                                                                     ```
    ```     master=oldrep                                                                                        ```
    ```    ,transaction=repdelta                                                                                 ```
    ```    ,key=rep_socs                                                                                         ```
    ```    );                                                                                                    ```
    ```                                                                                                          ```
    ``` proc sort data=oldrep out=ol;by rep_socs;run;/* oldrep has been updated  and now is eq```ual to newrep*/ ```
    ``` proc sort data=newrep out=nu;by rep_socs;run;                                                            ```
    ``` title "Indepth comparison Updated RDMS table with New Feed";                                             ```
    ```                                                                                                          ```
    ``` proc compare data=ol compare=nu;                                                                         ```
    ``` run;                                                                                                     ```
    ```                                                                                                          ```
