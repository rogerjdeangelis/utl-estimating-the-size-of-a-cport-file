# utl-estimating-the-size-of-a-cport-file
Estimating the size of a cport file with 10 million records using a sample of 1,000 records
    Estimating the size of a cport file with 10 million records using a sample of 1,000 records.                                        
                                                                                                                                        
    You do not need to create the full cport to get an estimate of its size.                                                            
                                                                                                                                        
            Method                                                                                                                      
               1. Sample every 10,000th record in the 10 million record SAS dataset this yeilds 1,000 records (0.07 seconds)            
                  Use SAS point (ie point=10,000 to 10,000,000 by 10000) to get the 1,000 records (very fast)                           
               2. Run SAS Cport on the 1,000 sample                                                                                     
               3. Get the size of the CPORT file                                                                                        
               4. Mutiply the size of the 10,000.                                                                                       
                                                                                                                                        
                  For checking the estimate                                                                                             
                                                                                                                                        
               5. Run cport on the full 10 million records                                                                              
               6. Compare the full size 2,352,739,840 bytes with 10,000 times the sample size                                           
                  10,000*237,520) 2,375,200,000.                                                                                        
               7. Use 7zip with ultra option on the full cport file to get the zipped size                                              
                                                                                                                                        
    github                                                                                                                              
    https://tinyurl.com/yc8ltvfc                                                                                                        
    https://github.com/rogerjdeangelis/utl-estimating-the-size-of-a-cport-file                                                          
                                                                                                                                        
                                                                                                                                        
    SAS Forum                                                                                                                           
    https://tinyurl.com/yan3o4u2                                                                                                        
    https://communities.sas.com/t5/Administration-and-Deployment/CPORT-Estimate-size/m-p/665189                                         
                                                                                                                                        
    Directory m:/xpt (see macro on end to get this output)                                                                              
                                                                                                                                        
                                                   FILE_SIZE_                                                                           
      File                    PATHNAME               _BYTES_          Estimate                                                          
                                                                                                                                        
     10 million cport   m:/xpt\have.cpt            2,352,739,840      2,375,200,000                                                     
                                                                                                                                        
     1,000 sample cport m:/xpt\hav_0001.cpt              237,520      2,375,200,000 = 10000*237520                                      
                                                                      1,000 records out of 10,000,000                                   
     7zip ultra option  m:/xpt\have.7z               535,663,827                                                                        
                                                                                                                                        
    /*                   _                                                                                                              
    (_)_ __  _ __  _   _| |_                                                                                                            
    | | `_ \| `_ \| | | | __|                                                                                                           
    | | | | | |_) | |_| | |_                                                                                                            
    |_|_| |_| .__/ \__,_|\__|                                                                                                           
      __    |_|_ _                                                                                                                      
     / _|_   _| | |   ___ _ __   ___  _ __| |_                                                                                          
    | |_| | | | | |  / __| `_ \ / _ \| `__| __|                                                                                         
    |  _| |_| | | | | (__| |_) | (_) | |  | |_                                                                                          
    |_|  \__,_|_|_|  \___| .__/ \___/|_|   \__|                                                                                         
                         |_|                                                                                                            
    */                                                                                                                                  
    data have;                                                                                                                          
                                                                                                                                        
      retain tpl 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';                                                                                          
                                                                                                                                        
      call streaminit(91818);                                                                                                           
                                                                                                                                        
      array c[10] $32 chr1-chr10;                                                                                                       
      array n[10] 8   num1-num10;                                                                                                       
                                                                                                                                        
      do rec=1 to 10000000;                                                                                                             
                                                                                                                                        
        do i=1 to 10;                                                                                                                   
                                                                                                                                        
           beg= int(13*rand('uniform'))+1;                                                                                              
           len= int(13*rand('uniform'))+1;                                                                                              
           c[i]=substr(tpl,beg,len);                                                                                                    
                                                                                                                                        
        end;                                                                                                                            
                                                                                                                                        
        do i=1 to 5;                                                                                                                    
                                                                                                                                        
           n[i]= floor(100*rand('uniform'));                                                                                            
                                                                                                                                        
        end;                                                                                                                            
                                                                                                                                        
        do i=6 to 10;                                                                                                                   
                                                                                                                                        
           n[i]= rand('uniform');                                                                                                       
                                                                                                                                        
        end;                                                                                                                            
                                                                                                                                        
        output;                                                                                                                         
                                                                                                                                        
      end;                                                                                                                              
                                                                                                                                        
      stop;                                                                                                                             
                                                                                                                                        
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
    /*                                                                                                                                  
    FULL Dataset                                                                                                                        
                                                                                                                                        
    Middle Observation(5000000 ) of have - Total Obs 10,000,000                                                                         
                                                                                                                                        
                                                                                                                                        
     -- CHARACTER  TYPE/LEN Sample value                                                                                                
    TPL            C26      ABCDEFGHIJKLMNOP                                                                                            
    CHR1           C32      KLMNOPQR                                                                                                    
    CHR2           C32      CDEF                                                                                                        
    CHR3           C32      DEFG                                                                                                        
    CHR4           C32      K                                                                                                           
    CHR5           C32      KLMNOPQRSTUV                                                                                                
    CHR6           C32      HIJKLMNOPQ                                                                                                  
    CHR7           C32      LMN                                                                                                         
    CHR8           C32      ABCDEFGHIJKL                                                                                                
    CHR9           C32      BCDE                                                                                                        
    CHR10          C32      FGHIJKLMNOPQR                                                                                               
    TOTOBS         C16      10,000,000                                                                                                  
                                                                                                                                        
     -- NUMERIC --                                                                                                                      
    NUM1           N8       32                                                                                                          
    NUM2           N8       87                                                                                                          
    NUM3           N8       74                                                                                                          
    NUM4           N8       66                                                                                                          
    NUM5           N8       81                                                                                                          
    NUM6           N8       0.3766137154                                                                                                
    NUM7           N8       0.3245304737                                                                                                
    NUM8           N8       0.3325034545                                                                                                
    NUM9           N8       0.3595492935                                                                                                
    NUM10          N8       0.2950838502                                                                                                
    I              N8       11                                                                                                          
    BEG            N8       6                                                                                                           
    LEN            N8       8                                                                                                           
    */                                                                                                                                  
                                                                                                                                        
    /*           _               _                                                                                                      
      ___  _   _| |_ _ __  _   _| |_                                                                                                    
     / _ \| | | | __| `_ \| | | | __|                                                                                                   
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                    
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                   
                    |_|                                                                                                                 
    */                                                                                                                                  
                                                                                                                                        
     Directory m:/xpt (see macro on end to get this output)                                                                             
                                                                                                                                        
                                                   FILE_SIZE_                                                                           
      File                    PATHNAME               _BYTES_          Estimate                                                          
                                                                                                                                        
     10 million cport   m:/xpt\have.cpt            2,352,739,840                                                                        
                                                                                                                                        
     1,000 sample cport m:/xpt\hav_0001.cpt              237,520      2,375,200,000 = 10000*237520                                      
                                                                      1,000 records out of 10,000,000                                   
    /*                                                                                                                                  
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                  
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|                                                                                                 
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                 
    | .__/|_|  \___/ \___\___||___/___/                                                                                                 
    |_|                                                                                                                                 
    */                                                                                                                                  
                                                                                                                                        
    * draw a 1,000 record sample from the full 10 million record dataset;                                                               
                                                                                                                                        
    data hav_0001;                                                                                                                      
      do pt=10000 to 10000000 by 10000;                                                                                                 
         set have point=pt;                                                                                                             
         output;                                                                                                                        
      end;                                                                                                                              
      stop;                                                                                                                             
    run;quit;                                                                                                                           
                                                                                                                                        
    /*                                                                                                                                  
    NOTE: The data set WORK.HAV_0001 has 1000 observations and 25 variables.                                                            
    NOTE: Compressing data set WORK.HAV_0001 decreased size by 37.50 percent.                                                           
          Compressed is 5 pages; un-compressed would require 8 pages.                                                                   
    NOTE: DATA statement used (Total process time):                                                                                     
          real time           0.07 seconds                                                                                              
                                                                                                                                        
    Middle Observation(5000000 ) of hav_0001 - Total Obs 1,000                                                                          
                                                                                                                                        
     -- CHARACTER Type/Len  Value                                                                                                       
                                                                                                                                        
    TPL           C26      ABCDEFGHIJKLMNOP                                                                                             
    CHR1          C32      KLMNOPQR                                                                                                     
    CHR2          C32      CDEF                                                                                                         
    CHR3          C32      DEFG                                                                                                         
    CHR4          C32      K                                                                                                            
    CHR5          C32      KLMNOPQRSTUV                                                                                                 
    CHR6          C32      HIJKLMNOPQ                                                                                                   
    CHR7          C32      LMN                                                                                                          
    CHR8          C32      ABCDEFGHIJKL                                                                                                 
    CHR9          C32      BCDE                                                                                                         
    CHR10         C32      FGHIJKLMNOPQR                                                                                                
    TOTOBS        C16      1,000                                                                                                        
                                                                                                                                        
                                                                                                                                        
     -- NUMERIC --                                                                                                                      
    NUM1          N8       32                                                                                                           
    NUM2          N8       87                                                                                                           
    NUM3          N8       74                                                                                                           
    NUM4          N8       66                                                                                                           
    NUM5          N8       81                                                                                                           
    NUM6          N8       0.3766137154                                                                                                 
    NUM7          N8       0.3245304737                                                                                                 
    NUM8          N8       0.3325034545                                                                                                 
    NUM9          N8       0.3595492935                                                                                                 
    NUM10         N8       0.2950838502                                                                                                 
    I             N8       11                                                                                                           
    BEG           N8       6                                                                                                            
    LEN           N8       8                                                                                                            
    */                                                                                                                                  
                                                                                                                                        
    * CPORT the 1,000 sample;                                                                                                           
                                                                                                                                        
    proc cport file= "m:/xpt/have.cpt" data=have;                                                                                       
    run;quit;                                                                                                                           
                                                                                                                                        
    * check sizes (macro onrnd);                                                                                                        
                                                                                                                                        
    %utl_dirContents(dir=m:/xpt,dsout=work.want);                                                                                       
                                                                                                                                        
    proc print data=want;                                                                                                               
    run;quit;                                                                                                                           
                                                                                                                                        
    /*                                                                                                                                  
                                                         FILE_SIZE_                                                                     
         PATHNAME          FILE_SEQ    RECFM    LRECL     _BYTES_                                                                       
                                                                                                                                        
    m:/xpt\have.7z             1         V       384     535663827                                                                      
    m:/xpt\have.cpt            2         V       384     2352739840                                                                     
    m:/xpt\hav_0001.cpt        3         V       384     237520                                                                         
    */                                                                                                                                  
                                                                                                                                        
    /*                                                                                                                                  
     _ __ ___   __ _  ___ _ __ ___                                                                                                      
    | `_ ` _ \ / _` |/ __| `__/ _ \                                                                                                     
    | | | | | | (_| | (__| | | (_) |                                                                                                    
    |_| |_| |_|\__,_|\___|_|  \___/                                                                                                     
                                                                                                                                        
    */                                                                                                                                  
                                                                                                                                        
    %macro utl_dirContents(                                                                                                             
     dir= /* directory name to process */                                                                                               
    , ext= /* optional extension to filter on */                                                                                        
    , dsout=work.dir_contents /* dataset name to hold the file names */                                                                 
    , attribs=Y /* get file attributes? (Y/N ) */                                                                                       
    );                                                                                                                                  
    %global _dir_fileN;                                                                                                                 
    %local _syspathdlim _exitmsg _attrib_vars;                                                                                          
    %* verify the required parameter has been provided. ;                                                                               
    %if %length(&dir) = 0 %then %do;                                                                                                    
     %let _exitmsg = %str(E)RROR: No directory name specified - macro will exit.;                                                       
     %goto finish;                                                                                                                      
    %end;                                                                                                                               
    %* verify existence of the requested directory name. ;                                                                              
    %if %sysfunc(fileexist(&dir)) = 0 %then %do;                                                                                        
     %let _exitmsg = %str(E)RROR: Specified input location, &dir., does not exist - macro                                               
    will exit.;                                                                                                                         
     %goto finish;                                                                                                                      
    %end;                                                                                                                               
    %* set the separator character needed for the full file path: ;                                                                     
    %* (backslash for Windows, forward slash for UNIX systems) ;                                                                        
    %if &sysscp = WIN %then %do;                                                                                                        
     %let _syspathdlim = \;                                                                                                             
    %end;                                                                                                                               
    %else %do;                                                                                                                          
     %let _syspathdlim = /;                                                                                                             
    %end;                                                                                                                               
    /*--- begin data step to capture names of all file names found in the specified                                                     
    directory. ---*/                                                                                                                    
    data &dsout(keep=file_seq basefile pathname);                                                                                       
     length basefile $ 40 pathname $ 1000 _msg $ 1000;                                                                                  
     /* Allocate directory */                                                                                                           
     rc=FILENAME('xdir', "&dir");                                                                                                       
                                                                                                                                        
     if rc ne 0 then do;                                                                                                                
     _msg = "E" || 'RROR: Unable to assign fileref to specified directory. ' ||                                                         
    sysmsg();                                                                                                                           
     go to finish_datastep;                                                                                                             
     end;                                                                                                                               
     /* Open directory */                                                                                                               
     dirid=DOPEN('xdir');                                                                                                               
     if dirid eq 0 then do;                                                                                                             
     _msg = "E" || 'RROR: Unable to open specified directory. ' || sysmsg();                                                            
     go to finish_datastep;                                                                                                             
     end;                                                                                                                               
     /* Get number of information items */                                                                                              
     nfiles=DNUM(dirid);                                                                                                                
     do j = 1 to nfiles;                                                                                                                
     basefile = dread(dirid, j);                                                                                                        
     pathname=strip("&dir") || "&_syspathdlim." || strip(basefile);                                                                     
                                                                                                                                        
     %if %length(&ext) %then %do;                                                                                                       
    /* scan the final "word" of the full file name, delimited by dot character. */                                                      
                                                                                                                                        
     ext = scan(basefile,-1,'.');                                                                                                       
     if ext="&ext." then do;                                                                                                            
     file_seq + 1;                                                                                                                      
     output;                                                                                                                            
     end;                                                                                                                               
     %end;                                                                                                                              
     %else %do;                                                                                                                         
     file_seq + 1;                                                                                                                      
     output;                                                                                                                            
     %end;                                                                                                                              
     end;                                                                                                                               
     /* Close the directory */                                                                                                          
     rc=DCLOSE(dirid);                                                                                                                  
     /* Deallocate the directory */                                                                                                     
     rc=FILENAME('xdir');                                                                                                               
                                                                                                                                        
     call symputx('_dir_fileN', file_seq);                                                                                              
     finish_datastep:                                                                                                                   
     if _msg ne ' ' then do;                                                                                                            
     call symput('_exitmsg', _msg);                                                                                                     
     end;                                                                                                                               
    run;                                                                                                                                
    %if %upcase(&attribs)=Y and &_dir_fileN > 0 %then %do;                                                                              
     data _file_attr(keep=file_seq basefile infoname infoval);                                                                          
     length infoname infoval $ 500;                                                                                                     
     set &dsout.;                                                                                                                       
    /* open each file to get the additional attributes available. */                                                                    
     rc=filename("afile", pathname);                                                                                                    
     fid=fopen("afile");                                                                                                                
    /* return the number of system-dependent information items available for the                                                        
    external file. */                                                                                                                   
     infonum=foptnum(fid);                                                                                                              
    /* loop to get the name and value of each information item. */                                                                      
     do i=1 to infonum;                                                                                                                 
     infoname=foptname(fid,i);                                                                                                          
     infoval=finfo(fid,infoname);                                                                                                       
     if upcase(infoname) ne 'FILENAME' then output;                                                                                     
     end;                                                                                                                               
     close=fclose(fid);                                                                                                                 
     run;                                                                                                                               
     /* transpose each information item into its own variable */                                                                        
     proc transpose data=_file_attr out=trans_attr(drop=_:) ;                                                                           
     by file_seq basefile ;                                                                                                             
     var infoval;                                                                                                                       
     id infoname;                                                                                                                       
     run;                                                                                                                               
     proc sql noprint;                                                                                                                  
     select distinct name into : _attrib_vars separated by ', '                                                                         
     from dictionary.columns                                                                                                            
     where memname='TRANS_ATTR' and upcase(name) not in('BASEFILE', 'FILE_SEQ')                                                         
     order by varnum;                                                                                                                   
     quit;                                                                                                                              
     /* merge back the additional attributes to the related file name. */                                                               
     data &dsout.;                                                                                                                      
     merge &dsout. trans_attr;                                                                                                          
     by file_seq basefile;                                                                                                              
     run;                                                                                                                               
     proc datasets nolist memtype=data lib=work;                                                                                        
     delete _file_attr trans_attr;                                                                                                      
     run;                                                                                                                               
     quit;                                                                                                                              
    %end;                                                                                                                               
    %if %length(&_exitmsg) = 0 %then                                                                                                    
    %let _exitmsg = NOTE: &dsout created with &_dir_fileN. file names ;                                                                 
    %if %length(&ext) %then                                                                                                             
    %let _exitmsg = &_exitmsg where extension is equal to &ext.;                                                                        
    %let _exitmsg = &_exitmsg from &dir..;                                                                                              
    %finish:                                                                                                                            
    %put &_exitmsg;                                                                                                                     
    %if %length(&_attrib_vars) ne 0 %then %do;                                                                                          
     %put;                                                                                                                              
     %put NOTE: File attributes were requested and have been added to &dsout.. Variable                                                 
    names are &_attrib_vars.;                                                                                                           
    %end;                                                                                                                               
    %mend utl_dirContents;                                                                                                              
                                                                                                                                        
                                                                                                                                        
    %utl_dirContents(dir=m:/xpt,dsout=work.want);                                                                                       
                                                                                                                                        
                                                                                                                                        
