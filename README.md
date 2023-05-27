# utl-sas-proc-transpose-in-sas-r-wps-python-native-and-sql-code
SAS proc transpose in sas r wps python native and sql code 

    %let pgm=utl-sas-proc-transpose-in-sas-r-wps-python-native-and-sql-code;

    SAS proc transpose in sas r wps python native and sql code

    github
    https://tinyurl.com/4faw6j7n
    https://github.com/rogerjdeangelis/utl-sas-proc-transpose-in-sas-r-wps-python-native-and-sql-code

       NON SQL

             1. SAS proc transpose
             2. WPS proc transpose
             3. WPS proc r native
                https://stackoverflow.com/users/21227705/mfg3z0
        SQL
             4. WPS proc r sqllite
             5. Python sqllite
             6. SAS sql no sql arrays
             7. SAS sql with sql arrays
             8. WPS sql no arrays

    There is an implementation of sql 'partitioning' in SAS below.
    Partitioning opens up sequential processing in SAS.
    Below I use SAS to add the concept of row_number by group.

    Each school tells how many vacancies they have for each subject.
    They can give up to four reasons for why they think they have so many vacancies.

    For example school 1 has given two reasons why English has so many vacancies (Pay and location)."

    stackoverflow r
    https://tinyurl.com/2azy99a2
    https://stackoverflow.com/questions/76335650/pivoting-reasons-into-their-own-columns-in-r

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    libname sd1 "d:/sd1";
    options validvarname=v7; /*---- IMPORTANT for R Python ----*/
    data sd1.have;informat
    school_id $1.
    subject $7.
    vacancies 3.
    reason $10.
    ;input
    school_id subject vacancies reason;
    cards4;
    1 English 40 Pay
    1 English 40 Location
    2 Maths 20 Pay
    3 English 10 Experience
    3 French 40 Pay
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                             |                                                          */
    /* Up to 40 obs from last table SD1.HAVE total obs=5           |  RULES                                                   */
    /*                                                             |                                                          */
    /*        school_                                              |  We want to place the mutiple reasons across             */
    /* Obs      id       subject    vacancies    reason            |                                                          */
    /*                                                             |          school_                                         */
    /*  1        1       English        40       Pay               |   Obs      id       subject    vacancies    reason       */
    /*  2        1       English        40       Location          |                                                          */
    /*  3        2       Maths          20       Pay               |    1        1       English        40       Pay          */
    /*  4        3       English        10       Experience        |    2        1       English        40       Location     */
    /*  5        3       French         40       Pay               |                                                          */
    /*                                                             |  So the two observations become one observation          */
    /* OUTPUT                                                      |                                                          */
    /*  school_id  subject    vacancies   Reason1      Reason2 ..  |   school                                                 */
    /*                                                             |   id   subject vacancies Reason1 Reason2 Reason3 Reason4 */
    /*  1          English    40          Pay            Location  |                                                          */
    /*  2          Maths      20          Pay                      |   1    English    40     Pay     Location                */
    /*  3          French     40          Pay                      |                                                          */
    /*  3          English    10          Experience               |                                                          */
    /*                                                             |                                                          */
    /**************************************************************************************************************************/

    /*                                         _
    / |  ___  __ _ ___   _ __  _ __ ___   ___ | |_ _ __ __ _ _ __  ___ _ __   ___  ___  ___
    | | / __|/ _` / __| | `_ \| `__/ _ \ / __|| __| `__/ _` | `_ \/ __| `_ \ / _ \/ __|/ _ \
    | | \__ \ (_| \__ \ | |_) | | | (_) | (__ | |_| | | (_| | | | \__ \ |_) | (_) \__ \  __/
    |_| |___/\__,_|___/ | .__/|_|  \___/ \___| \__|_|  \__,_|_| |_|___/ .__/ \___/|___/\___|
                        |_|                                           |_|
    */

    proc transpose data=sd1.have prefix=reason
         out=havxpo;
      by school_id subject vacancies;
      var reason;
    run;quit;

    /*___                                               _
    |___ \  __      ___ __  ___   _ __  _ __ ___   ___ | |_ _ __ __ _ _ __  ___ _ __   ___  ___  ___
      __) | \ \ /\ / / `_ \/ __| | `_ \| `__/ _ \ / __|| __| `__/ _` | `_ \/ __| `_ \ / _ \/ __|/ _ \
     / __/   \ V  V /| |_) \__ \ | |_) | | | (_) | (__ | |_| | | (_| | | | \__ \ |_) | (_) \__ \  __/
    |_____|   \_/\_/ | .__/|___/ | .__/|_|  \___/ \___| \__|_|  \__,_|_| |_|___/ .__/ \___/|___/\___|
                     |_|         |_|                                           |_|
    */

    %utl_submit_wps64('

    libname sd1 "d:/sd1";

    proc transpose data=sd1.have prefix=reason
         out=havxpo;
      by school_id subject vacancies;
      var reason;
    run;quit;

    proc print data=havXpo;
    run;quit;

    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*  Obs    school_id    subject    vacancies    _NAME_     reason1      reason2                                           */
    /*                                                                                                                        */
    /*   1         1        English        40       reason    Pay           Location                                          */
    /*   2         2        Maths          20       reason    Pay                                                             */
    /*   3         3        English        10       reason    Experience                                                      */
    /*   4         3        French         40       reason    Pay                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                                                 _   _
    |___ /  __      ___ __  ___   _ __  _ __ ___   ___   _ __  _ __   __ _| |_(_)_   _____
      |_ \  \ \ /\ / / `_ \/ __| | `_ \| `__/ _ \ / __| | `__|| `_ \ / _` | __| \ \ / / _ \
     ___) |  \ V  V /| |_) \__ \ | |_) | | | (_) | (__  | |   | | | | (_| | |_| |\ V /  __/
    |____/    \_/\_/ | .__/|___/ | .__/|_|  \___/ \___| |_|   |_| |_|\__,_|\__|_| \_/ \___|
                     |_|         |_|
    */

    %utl_submit_wps64('
     options validvarname=v7;
     libname sd1 "d:/sd1";
     proc contents data=sd1.have;
     run;quit;
     proc r;
     export data=sd1.have r=dat;
     submit;
     library(tidyverse);
     dat;
     want<-dat %>%
       group_by(school_id, subject) %>%
       mutate(reason_name = paste0("reason", row_number())) %>%
       ungroup() %>%
       pivot_wider(
         id_cols = c(school_id, subject, vacancies),
         values_from = reason,
         names_from = reason_name
       );
     endsubmit;
     import data=sd1.want_r_native r=want;
    run;quit;
    proc print data=sd1.want_r_native;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* bs    school_id    subject    vacancies     reason1      reason2                                                       */
    /*                                                                                                                        */
    /* 1         1        English        40       Pay           Location                                                      */
    /* 2         2        Maths          20       Pay                                                                         */
    /* 3         3        English        10       Experience                                                                  */
    /* 4         3        French         40       Pay                                                                         */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                                                                 _ _ _ _
    | || |  __      ___ __  ___   _ __  _ __ ___   ___   _ __   ___  __ _| | (_) |_ ___
    | || |_ \ \ /\ / / `_ \/ __| | `_ \| `__/ _ \ / __| | `__| / __|/ _` | | | | __/ _ \
    |__   _| \ V  V /| |_) \__ \ | |_) | | | (_) | (__  | |    \__ \ (_| | | | | ||  __/
       |_|    \_/\_/ | .__/|___/ | .__/|_|  \___/ \___| |_|    |___/\__, |_|_|_|\__\___|
                     |_|         |_|                                   |_|
    */

    %utl_wpsbegin;
    parmcards4;
    options validvarname=any;
    libname sd1 "d:/sd1";

    /*---- Simplifying the key just for easier documentation ----*/
    data sd1.havkey;
      length key $44;
      set sd1.have;
      key=catx("@",school_id,subject,vacancies);
      keep key reason;
    run;quit;

    proc r;
    export data=sd1.havkey r=havkey;
    submit;
    library(sqldf);
    part<-sqldf("
           select key, reason, row_number() OVER (PARTITION BY key) as part from havkey
           ");
    want_r<-sqldf("
       select
          key
         ,max(case when part=1 then reason else '' end) as reason1
         ,max(case when part=2 then reason else '' end) as reason2
         ,max(case when part=3 then reason else '' end) as reason3
         ,max(case when part=4 then reason else '' end) as reason4
       from
         part
       group
         by key
    ");
    want_r;
    endsubmit;
    import data=want_r r=want_r;
    proc print data=want_r;
    run;quit;
    ;;;;
    %utl_wpsend;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs        key          reason1      reason2     reason3    reason4                                                    */
    /*                                                                                                                        */
    /*  1     1@English@40    Pay           Location                                                                          */
    /*  2     2@Maths@20      Pay                                                                                             */
    /*  3     3@English@10    Experience                                                                                      */
    /*  4     3@French@40     Pay                                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                _   _                             _ _ _ _
    | ___|   _ __  _   _| |_| |__   ___  _ __    ___  __ _| | (_) |_ ___
    |___ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | | | | __/ _ \
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | | | | ||  __/
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|_|_|\__\___|
            |_|    |___/                                |_|
    */

    proc datasets lib=work kill nodetails nolist;
    run;quit;

    proc datasets lib=sd1 nodetails nolist;
     delete res;
    run;quit;

    %utlfkil(d:/xpt/res.xpt);

    libname sd1 "d:/sd1";

    data sd1.havkey;
      length key $44;
      set sd1.have;
      key=catx("@",school_id,subject,vacancies);
      keep key reason;
    run;quit;

    %utl_pybegin;
    parmcards4;
    from os import path
    import pandas as pd
    import xport
    import xport.v56
    import pyreadstat
    import numpy as np
    from pandasql import sqldf
    mysql = lambda q: sqldf(q, globals())
    from pandasql import PandaSQL
    pdsql = PandaSQL(persist=True)
    sqlite3conn = next(pdsql.conn.gen).connection.connection
    sqlite3conn.enable_load_extension(True)
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll')
    mysql = lambda q: sqldf(q, globals())
    havkey, meta = pyreadstat.read_sas7bdat("d:/sd1/havkey.sas7bdat")
    print(havkey);
    part= pdsql("""
           select key, reason, row_number() OVER (PARTITION BY key) as part from havkey
    """);
    print(part);
    res= pdsql("""
       select
          key
         ,max(case when part=1 then reason else '' end) as reason1
         ,max(case when part=2 then reason else '' end) as reason2
         ,max(case when part=3 then reason else '' end) as reason3
         ,max(case when part=4 then reason else '' end) as reason4
       from
         part
       group
         by key
    """);
    print(res);
    ds = xport.Dataset(res, name='res')
    with open('d:/xpt/res.xpt', 'wb') as f:
        xport.v56.dump(ds, f)
    ;;;;
    %utl_pyend;

    libname pyxpt xport "d:/xpt/res.xpt";

    proc contents data=pyxpt._all_;
    run;quit;

    proc print data=pyxpt.res;
    run;quit;

    data res;
       set pyxpt.res;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Obs        key         reason1       reason2     reason3    reason4                                                   */
    /*                                                                                                                        */
    /*    1    1@English@40    Pay           Location                                                                         */
    /*    2    2@Maths@20      Pay                                                                                            */
    /*    3    3@English@10    Experience                                                                                     */
    /*    4    3@French@40     Pay                                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*__                               _
     / /_    ___  __ _ ___   ___  __ _| |  _ __   ___     __ _ _ __ _ __ __ _ _   _ ___
    | `_ \  / __|/ _` / __| / __|/ _` | | | `_ \ / _ \   / _` | `__| `__/ _` | | | / __|
    | (_) | \__ \ (_| \__ \ \__ \ (_| | | | | | | (_) | | (_| | |  | | | (_| | |_| \__ \
     \___/  |___/\__,_|___/ |___/\__, |_| |_| |_|\___/   \__,_|_|  |_|  \__,_|\__, |___/
                                    |_|                                       |___/
    */
    libname sd1 "d:/sd1";

    proc datasets lib=worl nodetails nolist;
     delete want;
    run;quit;

    /*---- Simplifying the key just for easier documentation ----*/
    data sd1.havKey;
      length key $44;
      set sd1.have;
      key=catx("@",school_id,subject,vacancies);
      keep key reason;
    run;quit;

    proc sql;

       create view partition as
          select monotonic() as partition , key, reason from sd1.havkey where key="1@English@40"  union
          select monotonic() as partition , key, reason from sd1.havkey where key="2@Maths@20"    union
          select monotonic() as partition , key, reason from sd1.havkey where key="3@English@10"  union
          select monotonic() as partition , key, reason from sd1.havkey where key="3@French@40"
       ;
       create table want as select
          key
         ,max(case when partition=1 then reason else '' end) as reason1
         ,max(case when partition=2 then reason else '' end) as reason2
         ,max(case when partition=3 then reason else '' end) as reason3
         ,max(case when partition=4 then reason else '' end) as reason4
       from
         partition
       group
         by key
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Up to 40 obs from WANT total obs=4 26MAY2023:15:38:50                                                                 */
    /*                                                                                                                        */
    /*  Obs        key         reason1       reason2     reason3    reason4                                                   */
    /*                                                                                                                        */
    /*   1     1@English@40    Pay           Location                                                                         */
    /*   2     2@Maths@20      Pay                                                                                            */
    /*   3     3@English@10    Experience                                                                                     */
    /*   4     3@French@40     Pay                                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*____                             _
    |___  |  ___  __ _ ___   ___  __ _| |   __ _ _ __ _ __ __ _ _   _ ___
       / /  / __|/ _` / __| / __|/ _` | |  / _` | `__| `__/ _` | | | / __|
      / /   \__ \ (_| \__ \ \__ \ (_| | | | (_| | |  | | | (_| | |_| \__ \
     /_/    |___/\__,_|___/ |___/\__, |_|  \__,_|_|  |_|  \__,_|\__, |___/
                                    |_|                         |___/
    */

    libname sd1 "d:/sd1";

    proc datasets lib=worl nodetails nolist;
     delete want;
    run;quit;

    proc sql;
      select
        distinct key
      into
        :_key1 - :_key99
      from
        sd1.havkey
    ;quit;

    %let _keyn=&sqlobs ;

    %array(_seq,values=1-&_keyn);

    %put &_key2;  /*---- 2@Maths@20 ----*/
    %put &_seq2;  /*---- 2          ----*/
    %put &_keyn;  /*---- 4          ----*/

    proc sql;

       create view partition as
         %do_over(_keys,phrase=%str(
           select monotonic() as partition , key, reason from sd1.havkey where key="?"),between=union)
       ;
       create table want as select
           key
         %do_over(_seq,phrase=%str(
          ,max(case when partition=? then reason else '' end) as reason?))
       from
         partiti on
       group
         by key

    ;quit;

    %arraydelete(_key);
    %arraydelete(_seq);

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Up to 40 obs from WANT total obs=4 26MAY2023:15:38:50                                                                 */
    /*                                                                                                                        */
    /*  Obs        key         reason1       reason2     reason3    reason4                                                   */
    /*                                                                                                                        */
    /*   1     1@English@40    Pay           Location                                                                         */
    /*   2     2@Maths@20      Pay                                                                                            */
    /*   3     3@English@10    Experience                                                                                     */
    /*   4     3@French@40     Pay                                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                   _
     ( _ )  __      ___ __  ___   ___  __ _| |  _ __   ___     __ _ _ __ _ __ __ _ _   _ ___
     / _ \  \ \ /\ / / `_ \/ __| / __|/ _` | | | `_ \ / _ \   / _` | `__| `__/ _` | | | / __|
    | (_) |  \ V  V /| |_) \__ \ \__ \ (_| | | | | | | (_) | | (_| | |  | | | (_| | |_| \__ \
     \___/    \_/\_/ | .__/|___/ |___/\__, |_| |_| |_|\___/   \__,_|_|  |_|  \__,_|\__, |___/
                     |_|                 |_|                                       |___/
    */

    proc datasets lib=sd1 nodetails nolist;
     delete want_sas_no_ary havKey;
    run;quit;

    %utl_submit_wps64('

    options validvarname=any;

    libname sd1 "d:/sd1";

    /*---- Simplifying the key just for easier documentation ----*/
    data sd1.havKey;
      length key $44;
      set sd1.have;
      key=catx("@",school_id,subject,vacancies);
      keep key reason;
    run;quit;

    proc sql;

       create view partition as
          select monotonic() as partition , key, reason from sd1.havkey where key="1@English@40"  union
          select monotonic() as partition , key, reason from sd1.havkey where key="2@Maths@20"    union
          select monotonic() as partition , key, reason from sd1.havkey where key="3@English@10"  union
          select monotonic() as partition , key, reason from sd1.havkey where key="3@French@40"
    ;
       create table sd1.want_sas_no_ary as select
          key
         ,max(case when partition=1 then reason else "" end) as reason1
         ,max(case when partition=2 then reason else "" end) as reason2
         ,max(case when partition=3 then reason else "" end) as reason3
         ,max(case when partition=4 then reason else "" end) as reason4
       from
         partition
       group
         by key
    ;quit;

    proc print data=sd1.want_sas_no_ary ;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs        key          reason1      reason2     reason3    reason4                                                    */
    /*                                                                                                                        */
    /*  1     1@English@40    Pay           Location                                                                          */
    /*  2     2@Maths@20      Pay                                                                                             */
    /*  3     3@English@10    Experience                                                                                      */
    /*  4     3@French@40     Pay                                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
