# Examination-Management-System
This is my first Repository
<br>
Author - Shreya Patil
<br>
#EXAMINATION MANAGEMENT SYSTEM 
import sys
import mysql.connector
mycon=mysql.connector.connect(host='localhost',user='root', password='Shreya@22',database='exam1',autocommit=True)
mycur=mycon.cursor()
print("\t","="*80)
print("\t\t    WELCOME TO EXAMINATION MANAGEMENT SYSTEM")
print("\t","="*80)
print("\n")

#Choice 1
# To create profile
def Student_Profile():
    sql="Insert into student(adm_no,Name,class,section)values(%s,%s,%s,%s)"
    print('PLEASE PROVIDE THE REQUIRED INFORMATION\n')
    ad=input('ENTER THE ADMISSION NUMBER:')
    nm=input('ENTER THE STUDENT NAME:')
    o=[11,12]
    s=['A','B','C','D']
    while True:
        cls=int(input('ENTER THE CLASS(11/12):'))
        if cls not in o:
            print("Enter class from 11 and 12 only\n!!")
        else:
            break
    while True:
        sec=input('ENTER THE SECTION(A-D):').upper()
        if sec not in s:
            print('Enter section from (A-D) only\n !!')
        else:
            break
    value=(ad,nm,cls,sec)
    try:
        mycur.execute(sql,value)
        print(nm,'ADDED SUCCESSFULLY TO EXAM MODULE\n\n\n')
        mycon.commit()
    except:
        print('UNABLE TO INSERT!!!!! \n\n\n')


#Choice 2
# To edit name,section and class
def Edit_Profile():
    ph=input('ENTER THE ADMISSION NUMBER :')
    sql1='Select * from student where adm_no=%s'
    v1=(ph,)
    mycur.execute(sql1,v1)
    rec1=mycur.fetchone()
    if(rec1!=None):
        adm=rec1[0]
        name=rec1[1]
        cls=rec1[2]
        sec=rec1[3]
        print("\n Admission no. :",rec1[0])
        print("Name:" , rec1[1])
        print("Class:",rec1[2])
        print("Section:",rec1[3])
        print("\n Enter: ")
        print(" 1) To change Name")
        print(" 2) To change class ")
        print(" 3) To change section\n")
        ch=int(input('Enter choice (1-3):'))
        if ch==1:
            nm=input('\nENTER THE NEW NAME:')
            sq="Update student set name=%s where adm_no=%s"
            value=(nm,ph)
            try:
                mycur.execute(sq,value)
                mycon.commit()
                print("record updated\n\n\n")
            except:
                print("unable to update!\n\n\n")
        elif ch==2:
            nm=input('\nENTER THE NEW CLASS:')
            sq="Update student set class=%s where adm_no=%s"
            value=(nm,ph)
            try:
                mycur.execute(sq,value)
                mycon.commit()
                print("record updated\n\n\n")
            except:
                print("unable to update\n\n\n")
        elif ch==3:
            nm=input('\nENTER THE NEW SECTION:')
            sq="Update student set section=%s where adm_no=%s"
            value=(nm,ph)
            try:
                mycur.execute(sq,value)
                mycon.commit()
                print("record updated\n\n\n")
            except:
                print("unable to update\n\n\n")
        else:
            print("Enter valid choice to update\n\n\n")
    else:
        print("WRONG ADMISSION NUMBER!! \n")

#Choice 3
#Delete a student profile
def Remove_Profile():
    ph=input('\nENTER THE ADMISSION NUMBER TO DELETE:')
    sql1='Select * from student where adm_no=%s'
    v1=(ph,)
    mycur.execute(sql1,v1)
    rec1=mycur.fetchone()
    if(rec1!=None):
        adm=rec1[0]
        name=rec1[1]
        cls=rec1[2]
        sec=rec1[3]
        sql='Delete from student where Adm_no=%s'
        value=(ph,)
        try:
            mycur.execute(sql,value)
            mycon.commit()
            print('RECORD DELETED SUCCESSFULLY\n')
        except:
            mycon.rollback()
            print('UNABLE TO DELETE RECORD!!!\n')
    else:
        print("\nEnter admission number correctly!!! \n")


#Choice 4
#Enter marks and attendence of a student 
def Record_Entry():
    sql="Insert into RESULT(Adm_no,exam_name,sub1,sub2,sub3,sub4,sub5,total,percentage,attendance,grade,remarks) values(%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"
    print('\nPLEASE PROVIDE THE REQUIRED INFORMATION\n')
    ad=int(input('\nENTER THE ADMISSION NUMBER TO ENTER RECORD:'))
    nm=input('\nENTER THE EXAM NAME:')
    sub1=int(input('ENTER MARKS IN ENGLISH (MAX:100):'))
    sub2=int(input('ENTER MARKS IN PHYSICS (MAX:100):'))
    sub3=int(input('ENTER MARKS IN CHEMISTRY (MAX:100):'))
    sub4=int(input('ENTER MARKS IN MATHS (MAX:100):'))
    sub5=int(input('ENTER MARKS IN COMPUTER SCIENCE (MAX:100):'))
    total=sub1+sub2+sub3+sub4+sub5
    per=total//5
    wrkday=int(input('ENTER TOTAL NUMBER OF WORKING DAYS:'))
    present=int(input('ENTER NO OF DAYS PRESENT:'))
    att=present/wrkday*100
    att=int(att)
    if(per>=90):
        g='A'
        rem=('EXCELLENT PERFORMANCE!!')
    elif(per>=75 and per<90):
        g='B'
        rem=('VERY GOOD PERFORMANCE!!')
    elif(per>=55 and per<=75):
        g='C'
        rem=('SATISFACTORY PERFORMANCE!!')
    elif(per>=35 and per<55):
        g='D'
        rem=('AVERAGE PERFORMANCE!!')
    else:
        g='E'
        rem=('SCOPE FOR IMPROVEMENT!!')
    value=(ad,nm,sub1,sub2,sub3,sub4,sub5,total,per,att,g,rem)
    try:
        mycur.execute(sql,value)
        mycon.commit()
        print('\n RECORD ADDED SUCCESSFULLY \n\n\n')
    except:
        print('\n UNABLE TO INSERT!!!!!\n\n\n')


#Choice 5
#Generate report card
def report_card():
    rec1=[]
    ad=int(input('\nENTER THE ADMISSION NUMBER TO SEARCH:'))
    print("\n")
    sql1="Select * from student,result where student.adm_no=result.Adm_no and result.Adm_no={}".format(ad)
    mycur.execute(sql1)
    rec1=mycur.fetchone()
    print("\t","="*80)
    print("\t\t\t               D.A.V PUBLIC SCHOOL ")
    print("\t\t\t\t              NERUL ")
    print("\t","="*80)
    print("\n")
    print("\t\t\t\t              REPORT CARD ")
    print('\t Admission No.   : ',rec1[0])
    print('\n\t Name            : ',rec1[1])
    print("\n\t Class             : ",rec1[2])
    print("\n\t Section         : ", rec1[3])
    print("\n\t Exam            : ", rec1[5])
    print("\n\t   Subject                 \t Marks ")
    print("\t 1) English",                                            "\t\t",          rec1[6])
    print("\t 2) Physics",                                           "\t\t",          rec1[7])
    print("\t 3) Chemistry",                                           "\t\t",          rec1[8])
    print("\t 4) Maths",                           "\t\t\t",          rec1[9])
    print("\t 5) Computer Science",         "\t",          rec1[10])
    print("\n\t Total Marks :", rec1[11],end="\t\n")
    print("\n\t Percentage  :", rec1[12],end="%\n")
    print("\n\t Attendence :", rec1[13],end="\t\n")
    print("\n\t Grade          :", rec1[14],end="\t\n")
    print("\n\t Remarks      :", rec1[15],end="\n")
    print("\n")
    print("*"*115,"\n")
    
    

#Choice 6
#To remove record
def modify_marks():
    ph=input('\nENTER THE ADMISSION NUMBER TO MODIFY: ')
    sql2='Select * from result where adm_no={}'.format(ph)
    mycur.execute(sql2)
    rec2=mycur.fetchall()
    if rec2==None:
        print('\nWRONG ADMISSION NUMBER GIVEN!!!!!!\n')
    else:
        try:
            print("Modify marks for which subject (sub#) ? ")
            subj=input("Enter the subject name: ")
            mks=int(input("Enter new marks: "))
            sql="delete from result where Adm_no={}".format(subj,mks)
            mycur.execute(sql)
            mycon.commit()
            print('RECORD UPDATED SUCCESSFULLY\n\n\n')
        except:
            mycon.rollback()
            print('UNABLE TO UPDATE RECORD!!!\n\n\n')




#Choice 7
#To Search Student Details
def search_studentdetails():
    st=input("Enter admission number : ")
    sql1="Select * from student where adm_no={}".format(st)
    mycur.execute(sql1)
    T=mycur.fetchone()
    if T==[]:
        print("SORRY RECORD DOES NOT EXIST")
    else:
        print(T)
    
#Choice 8
#Exit
def close():
    print('\nTHANK YOU FOR USING THE APPLICATION\n\n')
    print("\t","="*80)
    sys.exit()
    print('WELCOME TO EXAMINATION MODULE SYSTEM FOR CLASS-XI & XII \n\n')

#Function
while True:
    print("Enter:")
    print('1) CREATE A STUDENT PROFILE ! ')
    print('2) EDIT A STUDENT PROFILE !')
    print('3) DELETE A STUDENT PROFILE !')
    print('4) MARKS AND ATTENDANCE ENTRY !')
    print('5) GENERATE REPORT CARD !')
    print('6) MODIFY MARKS !')
    print('7) SEARCH STUDENT DETAILS')
    print('8) EXIT ! \n')
    choice=int(input('ENTER YOUR CHOICE (1-8) :'))
    if choice==1:
        Student_Profile()
    elif choice==2:
        Edit_Profile()
    elif choice==3:
        Remove_Profile()
    elif choice==4:
        Record_Entry()
    elif choice==5:
        report_card()
    elif choice==6:
        modify_marks()
    elif choice==7:
        search_studentdetails()
    elif choice==8:
        close()
    else:
        print('\n ENTER A VALID CHOICE!!!\n')
