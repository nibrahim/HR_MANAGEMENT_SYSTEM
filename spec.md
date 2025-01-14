1. OBJECTIVE:

Generates visiting cards , QRCODE or leaves count for a list of employees provided in a CSV
file


2. INPUT:

1.1. Command-Line Arguments:
    1. In command line there are three choices initdb , load , create.
       
       1. initdb will create table in the database. You can specify the databse name using -db command.
        For this first you need to create a user manually. And using that username you can create table directly.
         
         : initdb -h for help  
             Example:
             usage: gen_vcard.py initdb [-h] [-u NAME] [-db DBNAME]

              options:
              -h, --help            show this help message and exit
              -u NAME, --name NAME  Add username
               -s DBNAME, --dbname DBNAME
                        Data base name

       2. load will insert data into the table in the  database.
         
         : load -h for help
          Example:
                options:
                -h, --help            show this help message and exit
                -i IPFILE, --ipfile IPFILE
                                      Name of input csv file
                -t TABLENAME, --tablename TABLENAME
                                      Specify your table name
                -u NAME, --name NAME  Add username
                -s DBNAME, --dbname DBNAME
                                      Data base name
                -e EMPLOYEE_ID, --employee_id EMPLOYEE_ID
                                      specify employee id
                -d DATE, --date DATE  specify data
                -r REASON, --reason REASON
                                      specify reason for leave
      * If you sppecify csv file the data will automatically insert to employees table
      * If you want to insert data to leaves use -t "leaves" then add datas 
                  for example : python3 genvcard.py load -s "hr" -t "leaves" -e 7 -d "2023-11-27" -r "fever"

    3. create will generate vcard , qrcode or leaves count of employee.
         
         : create  -h for help
         Example:
                usage: gen_vcard.py create [-h] [-u NAME] [-s DBNAME] [-d DIMENSION] [-b] [-l] [-e EMPLOYEE_ID]

                options:
                  -h, --help            show this help message and exit
                  -u NAME, --name NAME  Add username
                  -s DBNAME, --dbname DBNAME
                                        Data base name
                  -d DIMENSION, --dimension DIMENSION
                                        Change dimension of QRCODE
                  -b, --qr_and_vcard    Get qrcode along with vcard, Default - vcard only
                  -l, --leaves          Get leaves count as a text file
                  -e EMPLOYEE_ID, --employee_id EMPLOYEE_ID
                  -a, --all             Get data of all employee

        * Default is vcard only : python3 genvcard.py create -s "hr" -e <id>
        * If you give -l you will get leave count as text file : python3 genvcard.py create -s "hr"  -l -e <id>
        * -b for getting qr along with vcard : python3 genvcard.py create -s "hr"  -b -e <id>
        * For getting multiple person's data use : python3 genvcard.py create -s "hr"  -e 3 -e9 
        * For getting all employees data use : python3 genvcard.py create -s "hr"  -a
    2. Arguments: Add -h or --help for getting new arguments.
    
    3. For customizing qrcode size, it will only take numeric characters from 100 to 500
    
1.2. Csv file

     Each row in the csv_file should have the following columns

           : Williams,Annette,Psychiatrist,annet.willi@holloway.org,9305709284

             This is the last name, first name, designation, email address and
             phone number. 

    A sample input file `sample_employees.csv` is provided in the folder samplecsvs.


3. OUTPUT:

       Will generate one vCard and QRCODE file per row in the csv_file. The filename
       will be the first name in the row (e.g. foo.vcf/png). 
       All the files will be in the `output` directory(vcard , qrcode and leave data).

      * This is a sample vcard file
        
        BEGIN:VCARD
        VERSION:2.1
        N:Reeves;Anne
        FN:Anne Reeves
        ORG:Authors, Inc.
        TITLE:Visual merchandiser
        TEL;WORK;VOICE:666.808.0750x9935
        ADR;WORK:;;100 Flat Grape Dr.;Fresno;CA;95555;United States of America
        EMAIL;PREF;INTERNET:anne.reeve@davis.com
        REV:20150922T195243Z
        END:VCARD

        The vCard file will have email as file name.
        
        * The QRCODE will contains this data

        * The leaves count data:
          The leave data will be a csv file containing all the datas of employee

          example:   [NAME,EMAIL,DESIGNATION,LEAVES TAKEN,TOTAL LEAVES,REMAINING LEAVES]

4. EXECUTION:

The script can be executed from the command line using:
 
    python genvcard.py -h
    
    * For getting help menu
    
    example:  
            usage: gen_vcard.py [-h] [-v] {initdb,load,create} ...

            Generates vCards and QR codes from a CSV file and stores in a PostgreSQL database.

            positional arguments:
              {initdb,load,create}  Subcommands
                initdb              Initialize the PostgreSQL database and create table
                load                Load CSV file into the PostgreSQL database
                create              Initialize creating vcard and qrcode

            options:
              -h, --help            show this help message and exit
              -v, --verbose         Print detailed logging

Default dbname is your_db 
Default username is harish

You can Change this using -u and -db commands