# utl-number-of-shoppers-in-store-every_5-minutes-time-series
Number of Shoppers every 5 minutes between 8:30AM and 9:00AM
    Number of Shoppers every 5 minutes between 8:30AM and 9:00AM                                                       
                                                                                                                       
        Two Solutions ( I suggest you use R instead of my code)                                                        
                                                                                                                       
              a. sas proc sql                                                                                          
              b. R                                                                                                     
                                                                                                                       
         For some reason this appears to very complex, but perhaps it is just me?                                      
                                                                                                                       
    github                                                                                                             
    https://tinyurl.com/ycjq8q8t                                                                                       
    https://github.com/rogerjdeangelis/utl-number-of-shoppers-in-store-every_5-minutes-time-series                     
                                                                                                                       
    StackOverflow                                                                                                      
    https://tinyurl.com/ycq2k2zg                                                                                       
    https://stackoverflow.com/questions/62060183/how-to-get-people-in-the-store-at-every-5-minutes                     
                                                                                                                       
    Chinsoon12 profile                                                                                                 
    https://stackoverflow.com/users/1989480/chinsoon12    
    
    related github                                                                                                                                           
    https://tinyurl.com/y6hh73qt                                                                                                                     
    https://github.com/rogerjdeangelis/utl-identify-overlapping-intervals-with-multiple-drug-exposure   

                                                                                                                       
    *_                   _                                                                                             
    (_)_ __  _ __  _   _| |_                                                                                           
    | | '_ \| '_ \| | | | __|                                                                                          
    | | | | | |_) | |_| | |_                                                                                           
    |_|_| |_| .__/ \__,_|\__|                                                                                          
            |_|                                                                                                        
    ;                                                                                                                  
                                                                                                                       
    proc datasets lib=work kill;                                                                                       
    run;quit;                                                                                                          
                                                                                                                       
    %symdel min max;                                                                                                   
                                                                                                                       
    options validvarname=upcase;                                                                                       
    libname sd1 "d:/sd1";                                                                                              
                                                                                                                       
    data sd1.have (compress=no drop=min max);                                                                          
                                                                                                                       
     retain Shoppers;                                                                                                  
                                                                                                                       
     informat IN_TIME YMDDTTM20. OUT_TIME YMDDTTM20.;                                                                  
                                                                                                                       
     input Shoppers IN_TIME OUT_TIME;                                                                                  
                                                                                                                       
     if _n_=1 then do;                                                                                                 
         min=input("2017-11-01-08:30:00",YMDDTTM20.);                                                                  
         max=input("2017-11-01-09:00:00",YMDDTTM20.);                                                                  
         putlog min=;                                                                                                  
         call symputx('min',put(min,11.));                                                                             
         call symputx('max',put(max,11.));                                                                             
     end;                                                                                                              
                                                                                                                       
    cards4;                                                                                                            
    11 2017-11-01-08:30:35 2017-11-01-08:31:35                                                                         
    12 2017-11-01-08:31:35 2017-11-01-08:32:35                                                                         
    13 2017-11-01-08:32:35 2017-11-01-08:33:35                                                                         
    14 2017-11-01-08:33:35 2017-11-01-08:36:35                                                                         
    15 2017-11-01-08:35:36 2017-11-01-08:36:35                                                                         
    16 2017-11-01-08:36:35 2017-11-01-08:37:35                                                                         
    17 2017-11-01-08:37:35 2017-11-01-08:38:35                                                                         
    18 2017-11-01-08:38:35 2017-11-01-08:39:35                                                                         
    19 2017-11-01-08:39:35 2017-11-01-08:40:35                                                                         
    22 2017-11-01-08:46:44 2017-11-01-08:47:30                                                                         
    33 2017-11-01-08:47:16 2017-11-01-08:49:16                                                                         
    44 2017-11-01-08:48:29 2017-11-01-08:51:05                                                                         
    33 2017-11-01-08:49:25 2017-11-01-08:55:25                                                                         
    33 2017-11-01-08:56:25 2017-11-01-08:58:25                                                                         
    ;;;;                                                                                                               
    run;quit;                                                                                                          
                                                                                                                       
    * Range of time;                                                                                                   
                                                                                                                       
    %put &min;  * 2017-11-01-08:30;                                                                                    
    %put &max;  * 2017-11-01-09:00;                                                                                    
                                                                                                                       
                                                                                                                       
    * create range template;                                                                                           
                                                                                                                       
    data havFmt(keep=start end);                                                                                       
                                                                                                                       
      do minFiv=&min to &max-300 by 300;                                                                               
                                                                                                                       
         start  =  minFiv;                                                                                             
         end    =  minFiv+300;                                                                                         
         output;                                                                                                       
                                                                                                                       
      end;                                                                                                             
    run;quit;                                                                                                          
                                                                                                                       
    /*                                                                                                                 
    work.havfmt obs=6                                                                                                  
                                                                                                                       
      01NOV2017:08:30:00    01NOV2017:08:35:00                                                                         
      01NOV2017:08:35:00    01NOV2017:08:40:00                                                                         
      01NOV2017:08:40:00    01NOV2017:08:45:00                                                                         
      01NOV2017:08:45:00    01NOV2017:08:50:00                                                                         
      01NOV2017:08:50:00    01NOV2017:08:55:00                                                                         
      01NOV2017:08:55:00    01NOV2017:09:00:00                                                                         
    */                                                                                                                 
                                                                                                                       
                                                                                                                       
    *           _                                                                                                      
     _ __ _   _| | ___  ___                                                                                            
    | '__| | | | |/ _ \/ __|                                                                                           
    | |  | |_| | |  __/\__ \                                                                                           
    |_|   \__,_|_|\___||___/                                                                                           
                                                                                                                       
    ;                                                                                                                  
    /*                                                                                                                 
      SD1.HAVE total obs=13                                  | RULES                                                   
                                                             |                                                         
      SHOPPERS       IN_TIME                OUT_TIME         | Shoppers                                                
                                                             |                                                         
         11      2017-11-01 08:30:35    2017-11-01 08:31:35  | 4  8:30 to 8:35                                         
         12      2017-11-01 08:31:35    2017-11-01 08:32:35  |    Shoppers 11, 12, 13, and 14                          
         13      2017-11-01 08:32:35    2017-11-01 08:33:35  |                                                         
         14      2017-11-01 08:33:35    2017-11-01 08:36:35  |                                                         
                                                                                                                       
         15      2017-11-01 08:35:36    2017-11-01 08:36:35  | 6  8:35 to 8:40                                         
         16      2017-11-01 08:36:35    2017-11-01 08:37:35  |    14, 15, 16, 17, 18 , 19                              
         17      2017-11-01 08:37:35    2017-11-01 08:38:35  |                                                         
         18      2017-11-01 08:38:35    2017-11-01 08:39:35  |                                                         
         19      2017-11-01 08:39:35    2017-11-01 08:40:35  | 1  8:40 to 8:45                                         
                                                                  19                                                   
                                                                                                                       
         22      2017-11-01 08:46:44    2017-11-01 08:47:30  | 3  8:45 to 8:50                                         
         33      2017-11-01 08:47:16    2017-11-01 08:49:16  |    22, 33 and 44                                        
         44      2017-11-01 08:48:29    2017-11-01 08:51:05  |                                                         
                                                                                                                       
         33      2017-11-01 08:49:25    2017-11-01 08:55:25  | 2  8:50 to 8:55                                         
         33      2017-11-01-08:56:25    2017-11-01-08:58:25  |    33 and 44                                            
                                                                                                                       
                                                                                                                       
                                                             | 1  8:55 to 9:00                                         
                                                                  Shoppers 33 counted once                             
                                                                  EVEN THOUGH 33 LEFT AND CAME BACK                    
                                                                                                                       
    *            _               _                                                                                     
      ___  _   _| |_ _ __  _   _| |_                                                                                   
     / _ \| | | | __| '_ \| | | | __|                                                                                  
    | (_) | |_| | |_| |_) | |_| | |_                                                                                   
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                  
                    |_|                                                                                                
     ___  __ _ ___                                                                                                     
    / __|/ _` / __|                                                                                                    
    \__ \ (_| \__ \                                                                                                    
    |___/\__,_|___/                                                                                                    
                                                                                                                       
    */                                                                                                                 
                                                                                                                       
                                                                                                                       
    WANT total obs=6                                                                                                   
                                                                                                                       
    Obs    SHOPPER          START                  END                                                                 
                                                                                                                       
     1        4       01NOV2017:08:30:00    01NOV2017:08:35:00                                                         
     2        6       01NOV2017:08:35:00    01NOV2017:08:40:00                                                         
     3        1       01NOV2017:08:40:00    01NOV2017:08:45:00                                                         
     4        3       01NOV2017:08:45:00    01NOV2017:08:50:00                                                         
     5        2       01NOV2017:08:50:00    01NOV2017:08:55:00                                                         
     6        1       01NOV2017:08:55:00    01NOV2017:09:00:00                                                         
                                                                                                                       
    *_        ____                                                                                                     
    | |__    |  _ \                                                                                                    
    | '_ \   | |_) |                                                                                                   
    | |_) |  |  _ <                                                                                                    
    |_.__(_) |_| \_\                                                                                                   
                                                                                                                       
    ;                                                                                                                  
                                                                                                                       
    WANT total obs=6                                                                                                   
                                                                                                                       
    Obs    SHOPPERS    CHECKIN    CHECKOUT                                                                             
                                                                                                                       
     1         4        8:30        8:35                                                                               
     2         6        8:35        8:40                                                                               
     3         1        8:40        8:45                                                                               
     4         3        8:45        8:50                                                                               
     5         2        8:50        8:55                                                                               
     6         1        8:55        9:00                                                                               
                                                                                                                       
    *          _       _   _                                                                                           
     ___  ___ | |_   _| |_(_) ___  _ __  ___                                                                           
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|                                                                          
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \                                                                          
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/                                                                          
      __ _     ___  __ _ ___                                                                                           
     / _` |   / __|/ _` / __|                                                                                          
    | (_| |_  \__ \ (_| \__ \                                                                                          
     \__,_(_) |___/\__,_|___/                                                                                          
                                                                                                                       
    ;                                                                                                                  
                                                                                                                       
    proc sql;                                                                                                          
      create                                                                                                           
         table want as                                                                                                 
      select                                                                                                           
         count(distinct r.shoppers) as shopper                                                                         
        ,l.start     format=datetime20.                                                                                
        ,l.end       format=datetime20.                                                                                
      from                                                                                                             
        havFmt as l full outer join sd1.have as r                                                                      
      on                                                                                                               
         (r.in_time    between l.start and l.end)  or                                                                  
         (r.out_time   between l.start and l.end)  or                                                                  
                                                                                                                       
         /* you need this for the 8:50 to 8:55 where you pick up an imbeded intervals ? */                             
         ( (l.start between r.in_time and (r.in_time + 300) ) and ( (r.in_time + 300) <= r.out_time) ) or              
         ( (l.end between r.out_time and (r.out_time + 300) ) and ( (r.out_time + 300) >= r.in_time) )                 
      group                                                                                                            
        by l.start, l.end                                                                                              
                                                                                                                       
    *_        ____                                                                                                     
    | |__    |  _ \                                                                                                    
    | '_ \   | |_) |                                                                                                   
    | |_) |  |  _ <                                                                                                    
    |_.__(_) |_| \_\                                                                                                   
                                                                                                                       
    ;                                                                                                                  
                                                                                                                       
    ;quit;                                                                                                             
                                                                                                                       
    %utlfkil(d:/xpt/want.xpt);                                                                                         
                                                                                                                       
    %utl_submit_r64(resolve('                                                                                          
    library(data.table);                                                                                               
    library(SASxport);                                                                                                 
    library(haven);                                                                                                    
    DT1<-data.table(read_sas("d:/sd1/have.sas7bdat"));                                                                 
    times <- seq(&min,&max,by=300);                                                                                    
    DT2 <- data.table(IN_TIME=times[-length(times)], OUT_TIME=times[-1L], key=c("IN_TIME","OUT_TIME"));                
    head(DT2);                                                                                                         
    setkey(DT1, IN_TIME, OUT_TIME);                                                                                    
    want<-foverlaps(DT2, DT1)[!is.na(SHOPPERS), uniqueN(SHOPPERS), .(i.IN_TIME, i.OUT_TIME)];                          
    write.xport(want,file="d:/xpt/want.xpt");                                                                          
    '));                                                                                                               
                                                                                                                       
    libname xpt xport "d:/xpt/want.xpt";                                                                               
    data want ;                                                                                                        
      retain shoppers;                                                                                                 
      format checkin checkout  hhmm5.;                                                                                 
      set xpt.want(rename=v1=Shoppers);;                                                                               
      checkin  = timepart(I_IN_TIM);                                                                                   
      checkout = timepart(I_OUT_TI);                                                                                   
      keep Shoppers checkin checkout;                                                                                  
    run;quit;                                                                                                          
    libname xpt clear;                                                                                                 
                                                                                                                       
