//-------------header files-----------------
    #include<fstream.h>
    #include<conio.h>
    #include<process.h>
    #include<ctype.h>
    #include<stdio.h>
    #include<dos.h>
    #include<string.h>
    #include<graphics.h>
    #include<math.h>
    #include<stdlib.h>
    #include<time.h>
    #define DOTCOLOR 14
    #define DIGCOLOR 4


 /*-------------------------Prototypes-----------------------*/
 /*----------------------------------------------------------*/
    void menu();
    void passw();
    void exit_fun();
    void line(int,char);
    void sysdate();
    void systime();
    void submenu_todo();
    void submenu_sch();
    void submenu_c();
    void submenu_q();
    void clockmenu();
    int arrowmenu_todo();
    int arrowsubmenu_todo();
    int arrowsubmenu_sch();
    int arrowsubmenu_c();
    int arrowsubmenu_q();
    int arrowmenuc1();
    int arrowmenu();
    void inp_convert();
    int evaluate(char *);
    /*-------------------------Function for line---------------------*/
void line(int b,char a)
{
      for(int i=0;i<=b;i++)
      {
	cout<<a;
      }
	cout<<"\n";
}
	//---------------------Function for Time-------------------//
 void systime()
{
	struct time t;
	line(78,'-');
	gettime(&t);
	printf("  %2d : %02d",t.ti_hour,t.ti_min);
	cout<<"  \t  Digital Diary By Manish";
}
	//---------------------Function for date-------------------//
void sysdate()
{
     struct date d;
     getdate(&d);
     printf("   \t   %d/%d/%d ",d.da_day,d.da_mon,d.da_year);
     cout<<"\n";
     line(78,'-');
}
/*-------------------------------CLASSES---------------------------*/

class contact
{
	char telephone[9],mobile1[15],mobile2[15],email[20];
	char name[30],fathername[20],address[30],pincode[8],job[20],
	company[30];

	public:
	void cgetdata();
	void cdisplaydata();

  char *return_telephone()
  {
	return telephone;
  }

  char *return_name()
  {
	return name;
   }
  char *return_address()
  {
	return address;
  }
  char *return_mobile1()
  {
	return mobile1;
  }
  char *return_mobile2()
  {
	return mobile2;
  }
  char *return_company()
  {
	return company;
  }
};

	//-------------------Structure Date1---------------------//

struct Date1
{
	int dd,mm,yy;
	int hr,min;
};

	//-------------------Functions of contact----------------//

void contact :: cgetdata()  //function to get data for contact
 {
	clrscr();
	systime();
	sysdate();
	cout<<"\n\t\t\t\t contact: ";
	cout<<"\n\nNAME: ";
	gets(name);
	cout<<"\nTELEPHONE no. ";
	gets(telephone);
	cout<<"\nMobile No.1: ";
	gets(mobile1);
	cout<<"\nMobile No.2: ";
	gets(mobile2);
	cout<<"\nEmail ID: ";
	gets(email);
	cout<<"\nAddress: ";
	gets(address);
	cout<<"\nPincode: ";
	gets(pincode);
	cout<<"\nJob: ";
	gets(job);
	cout<<"\nCompany: ";
	gets(company);
 }

	//------------------------------------------------------

void contact :: cdisplaydata()
 {
	clrscr();
	systime();
	sysdate();

	 cout<<"\n\t\t\t\t contact: ";
	 cout<<"\n\nNAME: ";
	 puts(name);
	 cout<<"\nTELEPHONE no.:  ";
	 puts(telephone);
	 cout<<"\nMobile No.1: ";
	 puts(mobile1);
	 cout<<"\nMobile No.2: ";
	 puts(mobile2);
	 cout<<"\nEmail ID: ";
	 puts(email);
	 cout<<"\nAddress: ";
	 puts(address);
	 cout<<"\nPincode: ";
	 puts(pincode);
	 cout<<"\nJob: ";
	 puts(job);
	 cout<<"\nCompany: ";
	 puts(company);
 }
	/* Contact Functions -> add,modify,delete,display */

void cadd()
{
	 clrscr();
	 fstream file;
	 file.open("contact.dat",ios::app|ios::binary);
	 contact c;
	 char rep;
	 do
	 {
		 c.cgetdata();
		 file.write((char*)&c,sizeof(c));
		 cout<<"\n Want to add more records(y/n)";
		 cin>>rep;
	 }while(rep=='y'||rep=='Y');
}
//---------------------------------------------------------

void cdisplay()
 {
	 clrscr();
	 fstream file;
	 file.open("contact.dat",ios::in|ios::binary);
	 contact c;
	 while(file.read((char*)&c,sizeof(c)))
	 {
		 c.cdisplaydata();
		 getch();
	 }

}

//---------------------------------------------------------
void cdelete()
 {
	 clrscr();
	 systime();
	 sysdate();
	 fstream file,file2;
	 file.open("contact.dat",ios::in|ios::binary);
	 file2.open("Ncontact.dat",ios::out|ios::binary);
	 contact c;
	 char name[20],mobile[10],rep;
	 cout<<"\nEnter name and mobile 1 for deletion: "
	     <<"\n name : ";
	 gets(name);
	 cout<<"\n Mobile 1: ";
	 gets(mobile);
	 while(file.read((char*)&c,sizeof(c)))
	 {
	  if(strcmp(c.return_name(),name)!=0 && strcmp(c.return_mobile1(),mobile)!=0)
	 {
		file2.write((char*)&c,sizeof(c));
	 }
	else
	 {
		 clrscr();
		 c.cdisplaydata();
		 cout<<"\n Are you sure you want to delete (y/n): ";
		 cin>>rep;
		 if(rep=='n'||rep=='N')
		 file2.write((char*)&c,sizeof(c));
	 }
	 }
	 file.close();
	 file2.close();
	 remove("contact.dat");
	 rename("Ncontact.dat","contact.dat");
 }
 //--------------------------------------------------
 void cmodify()
 {
	 clrscr();
	 char rep;
	 systime();
	 sysdate();
	 fstream file,file2 ;
	 file.open("contact.dat",ios::in|ios::binary);
	 file2.open("Ncontact.dat",ios::out|ios::binary);
	 contact c;
	 char name[20],mobile[10];
	 cout<<"\nEnter name and mobile 1 for modification: "
	     <<"\n name : ";
	 gets(name);
	 cout<<"\n Mobile 1: ";
	 gets(mobile);
	 while(file.read((char*)&c,sizeof(c)))
	 {
	  if(strcmp(c.return_name(),name)!=0 && strcmp(c.return_mobile1 (),mobile)!=0 )
	 {
		 file2.write((char*)&c,sizeof(c));
	 }
	 else
	 {
		 clrscr();
		 c.cdisplaydata();
		 cout<<"\n Are you sure you want to modify (y/n): ";
		 cin>>rep;
		 if(rep=='n'||rep=='N')
		 file2.write((char*)&c,sizeof(c));
		 else
	 {
		 clrscr();
		 c.cgetdata();
		 file2.write((char*)&c,sizeof(c));
	 }
		 file.close();
		 file2.close();
		 remove("contact.dat");
		 rename("Ncontact.dat","contact.dat");
 }
 }
 }
 //----------------------------------------------------------
void cphone_disp()
{
	 clrscr();
	 systime();
	 sysdate();
	 char tele[9];
	 cout<<"\nEnter the telephone no to search: ";
	 gets(tele);
	 fstream file;
	 file.open("contact.dat",ios::in|ios::binary);
	 contact c;
	 while(file.read((char*)&c,sizeof(c)))
	 {
	 if(strcmp(tele,c.return_telephone())==0)
	 {
	   clrscr();
	   line(78,'*') ;
	   cout<<"\t\t\t    Found!!"<<endl;
	   line(78,'*');
	   c.cdisplaydata();
	   getch();
	   }
	   else
	   {
		clrscr();
		systime();
		sysdate();
		cout<<"\n\n\n\n\n\n\n\n";
		line(78,'*');
		cout<<"\t\t\t No such contact exist\n";
		line(78,'*');
	   }
 }
}
//--------------------------------------------------------
void cname_disp()
{
	 clrscr();
	 systime();
	 sysdate();
	 char name[30];
	 cout<<"Enter the Name to search: ";
	 gets(name);
	 fstream file;
	 file.open("contact.dat",ios::in|ios::binary);
	 contact c;
	while(file.read((char*)&c,sizeof(c)))
	 {
	  if(strcmp(name,c.return_name())==0)
	{
		clrscr();
		line(78,'*');
		cout<<"\t\t\t    Found!!"<<endl;
		line(78,'*');
		c.cdisplaydata();
		getch();
	 }
	 else
	 {
	 clrscr();
	 systime();
	 sysdate();
	 cout<<"\n\n\n\n\n\n\n\n";
	 line(78,'*');
	 cout<<"\t\t\t No such contact exist\n";
	 line(78,'*');
	 }
 }
}
//--------------------------------------------------------
void caddress_disp()
{
	 clrscr();
	 systime();
	 sysdate();
	 char address[9];
	 cout<<"Enter the address to search: ";
	 gets(address);
	 fstream file;
	 file.open("contact.dat",ios::in|ios::binary);
	 contact c;
	 while(file.read((char*)&c,sizeof(c)))
	 {
	  if(strcmp(address,c.return_address())==0)
	 {
	 clrscr();
	 line(78,'*');
	 cout<<"\t\t\t    Found!!"<<endl;
	 line(78,'*');
	 c.cdisplaydata();
	 getch();
	 }
	 else
	 {
	 clrscr();
	 systime();
	 sysdate();
	 cout<<"\n\n\n\n\n\n\n\n";
	 line(78,'*');
	 cout<<"\t\t\t No such contact exist\n";
	 line(78,'*');
	 }
	 }
}
//--------------------------------------------------------
void cmobile_disp()
{
	 clrscr();
	 systime();
	 sysdate();
	 char mobile[9];
	 cout<<"Enter the telephone no to search: ";
	 gets(mobile);
	 fstream file;
	 file.open("contact.dat",ios::in|ios::binary);
	 contact c;
	 while(file.read((char*)&c,sizeof(c)))
	 {
	  if(strcmp(mobile,c.return_mobile1())==0||strcmp(mobile,c.return_mobile2())==0)
	 {
	 clrscr();
	 line(78,'*');
	 cout<<"\t\t\t    Found!!"<<endl;
	 line(78,'*');
	 c.cdisplaydata();
	 getch();
 }
	 else
	 {
	 clrscr();
	 systime();
	 sysdate();
	 cout<<"\n\n\n\n\n\n\n\n";
	 line(78,'*');
	 cout<<"\t\t\t No such contact exist\n";
	 line(78,'*');
	 }
 }
}
//--------------------------------------------------------
void ccompany_disp()
{
	 clrscr();
	 systime();
	 sysdate();
	 char company[9];
	 cout<<"Enter the Company for search: ";
	 gets(company);
	 fstream file;
	 file.open("contact.dat",ios::in|ios::binary);
	 contact c;
	 while(file.read((char*)&c,sizeof(c)))
	 {
	 if(strcmp(company,c.return_company())==0)
	 {
	 clrscr();
	 line(78,'*');
	 cout<<"\t\t\t    Found!!"<<endl;
	 line(78,'*');
	 c.cdisplaydata();
	 getch();
  }
	else
	{
	 clrscr();
	 systime();
	 sysdate();
	 cout<<"\n\n\n\n\n\n\n\n";
	 line(78,'*');
	cout<<"\t\t\t No such contact exist\n";
	line(78,'*');
	 }
 }
}
	//-----------------CLASS SCHEDULER(FOR SCHEDULE)------------//

class scheduler
{
	public:
	Date1 d1;
	char sch_name[30],job[40],cho,ch,rep;
	public:
	void sgetdata();
	void sdisplaydata();
	int check_dt(Date1 d2)
	{
	if(d1.dd==d2.dd&&d1.mm==d2.mm&&d1.yy==d2.yy)
	 return 1;
	 else
	 return 0;
	 }
  char *return_name()
  {
	return sch_name;
  }
  char *return_job()
  {
	return job;
  }
  };
  //-------------------FUNCTIONS FOR SCHEDULER-----------------------------------
void scheduler::sgetdata()
{
   char str[30];
   clrscr();
   systime();
   sysdate();
   cout<<"\n\n";
   line(78,'+');
   cout<<"DATE"<<" (Day/Month/year) : ";
   gets(str);
   sscanf(str,"%2d/%2d/%4d",&d1.dd,&d1.mm,&d1.yy);
   printf("%d/%d/%d",d1.dd,d1.mm,d1.yy);
   cout<<"\n";
   line(78,'+');
   cout<<"\n\n";
   line(78,'+');
   cout<<" HOUR:Min : ";
   gets(str);
   sscanf(str,"%2d:%2d",&d1.hr,&d1.min);
   printf("%d:%d",d1.hr,d1.min);
   cout<<"\n";
   line(78,'+');
   cout<<"\n Title : ";
   gets(sch_name);
   cout<<"\nDescription :";
   gets(job);

}

   //------------------------------------------------------
void scheduler::sdisplaydata()
{
	   clrscr();
	   systime();
	   sysdate();
	   cout<<"\n\n";
	   line(78,'+');
	   cout<<"DATE"<<" (Day/Month/year) : ";
	   printf("%2d/%2d/%4d",d1.dd,d1.mm,d1.yy);
	   cout<<"\n";
	   line(78,'+');
	   cout<<"\n\n";
	   line(78,'+');
	   cout<<" HOUR/Min : ";
	   printf("%2d:%2d",d1.hr,d1.min);
	   cout<<"\n";
	   line(78,'+');
	   cout<<"\n\n";
	   line(78,'+');
	   cout<<"\n Title : ";
	   puts(sch_name);
	   cout<<"\nDescription :";
	   puts(job);
	   getch();
}
//---------------------------------------------
void sadd()
{
	   clrscr();
	   fstream file;
	   file.open("scheduler.dat",ios::app|ios :: binary);
	   scheduler sc;
	   char rep;
	   do
	   {
		   sc.sgetdata();
		   file.write((char*)&sc,sizeof(sc));
		   cout<<" \n\n\t Want to add more records(y/n) : ";
		   cin>>rep;
	   }while(rep=='y'||rep=='Y') ;
}

//-----------------------------------------------
void sdisplayall()
{
	clrscr();
	fstream file;
	file.open("scheduler.dat",ios::in|ios::binary);
	scheduler sc;
	while(file.read((char*)&sc,sizeof(sc)))
	{
		clrscr();
		sc.sdisplaydata();
		getch();
		}
		}
   //------------------------------------------------
void sdelete()
{
   Date1 d;
   clrscr();
   systime();
   sysdate();
   char rep;
   fstream fin,fout;
   fin.open("scheduler.dat",ios::in|ios::binary);
   fout.open("Nscheduler.dat",ios::out|ios::binary);
   scheduler sc;
   cout<<"\n\n";
   line(78,'+');
   cout<<"Enter the date of that schedule which you want delete:"
       <<"\nt Day = ";
   cin>>d.dd;
   cout<<"\t Month =";
   cin>>d.mm;
   cout<<"\t Year = ";
   cin>>d.yy;
   cout<<"\n";
   line(78,'+');
   while(fin.read((char*)&sc,sizeof(sc)))
   {
     if(sc.check_dt(d)==1)
      {
	  clrscr();
	  sc.sdisplaydata();
	  getch();
	  cout<<"\n\t Is this schedule you want to delete: ";
	  cin>>rep;
	  if(rep=='y'|| rep=='Y')
	  {
		clrscr();
		systime();
		sysdate();
		line(78,'*');
		cout<<"\t\t  DELETED!! \n";
		line(78,'*');
		}

	  else
	  {
		cout<<"\n\t\t  No Problem  ";
		fout.write((char*)&sc,sizeof(sc));
		}
		}
		else
		fout.write((char*)&sc,sizeof(sc));
		fin.close();
		fout.close();
		remove("scheduler.dat");
		rename("Nscheduler.dat","schedular.dat");
		getch();
		}
	 }
	 //-----------------------------------------------------------
void smodify()
{
	int flag;
	clrscr();
	systime();
	sysdate();
	fstream fin,fout;
	fin.open("scheduler.dat",ios::in|ios::binary);
	fout.open("Nscheduler.dat",ios::out|ios::binary);
	scheduler sc;
	Date1 d;
	cout<<"\n\n";
	line(78,'*');
	 cout<<"Enter the date of that schedule which you want delete:"
       <<"\nt Day = ";
       cin>>d.dd;
       cout<<"\t Month =";
       cin>>d.mm;
       cout<<"\t Year = ";
       cin>>d.yy;
       cout<<"\n";
       line(78,'+');
       while(fin.read((char*)&sc,sizeof(sc)))
       {
       if(sc.check_dt(d)==1)
       {
	   flag=1;
	   }
       else
       {
       fout.write((char*)&sc,sizeof(sc));
       }
       }
       if(flag==1)
       {
	clrscr();
	gotoxy(10,5);
	cout<<"Enter the data in schedule  to be modified \n\n:";
	sc.sgetdata();
	getch();
	fout.write((char*)&sc,sizeof(sc));
	cout<<"\n\n\t\t     Your Record has been modified!! ";
	}
	if(flag==0)
	{
	clrscr();
	systime();
	sysdate();
	cout<<"\n\n\n\n\n\n\n\n";
	line(78,'*');
	cout<<"\t\t\t No such Schedule exist \n";
	line(78,'*');
	}
	fin.close();
	fout.close();
	remove("scheduler.dat");
	rename("Nscheduler.dat","scheduler.dat");
	getch();
	}
	//------------------------------------------------------------------
void ssview()
	{
	scheduler sc;
	clrscr();
	systime();
	sysdate();
	Date1 d;
	int flag=0;
	ifstream fin("scheduler.dat",ios::in | ios::binary);
	cout<<"\n\n";
	line(78,'+');
	cout<<" Enter the date of that schedule which you want to delete"
	    <<"\nt Day = ";
       scanf("%2d",&d.dd);
       cout<<"\t Month =";
       scanf("%2d",&d.mm);
       cout<<"\t Year = ";
       scanf("%4d",&d.yy);
       cout<<"\n";
       line(78,'+');
       cout<<d.dd<<"/"<<d.mm<<"/"<<d.yy;
       while(fin.read((char*)&sc,sizeof(sc)))
       {
       cout<<sc.d1.dd<<sc.d1.mm<<sc.d1.yy;
       getch();
       if(sc.check_dt(d)==1)
       {
	  clrscr();
	  sc.sdisplaydata();
	  getch();
	  flag=1;
	  }}
	  if(flag!=1)
	  {
	  clrscr();
	  systime();
	  sysdate();
	  cout<<"\n\n\n\n\n\n\n\n";
	  line(78,'*');
	  cout<<"\t\t\t No such schedule exist \n";
	  line(78,'*');
	 }
	 getch();
	 fin.close();
	 }
//------------------------------CLASS To_do_list----------------------
struct Date2
{
	long int dd,mm,yy;
	long int hr,min;
	};
//--------------------------------------------------------
class to_do_list
{
	Date2 d1;
	int size;
	char job_to_do[20][20];
	public:
	void tgetdata();
	void tdisplaydata();
	int check_dt(Date2 d2)
	{
		if(d1.dd==d2.dd && d1.mm==d2.mm && d1.yy==d2.yy)
		return 1;
		else return 0;
		}
		};
//-------------------------FUNCTIONS FOR TO_DO_LIST-------------------------
void to_do_list::tgetdata()
{
	 char rep;
	 char str[30];
	 clrscr();
	 systime();
	 sysdate();
	 cout<<"\n\n";
	 line(78,'+/');
	 cout<<"DATE   "<<"  (Day/Month/year) :   " ;
	 gets(str);
	 sscanf(str,"%1d/%1d/%1d",&d1.dd,&d1.mm,&d1.yy);
	 cout<<"\n";
	 line(78,'*');
	 for(int i=0;i<20;i++)
	 {
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\t\t  Enter the Task you want to do: \n";
		 line(78,'+');
		 cout<<"\n\t\t TASK "<<i+1<<"  : ";
		 gets(job_to_do[i]);
		 cout<<"\n";
		 line(78,'+');
		 cout<<"\n Want to Add more task?? (y/n)  : ";
		 cin>>rep;
		 if(rep=='n'||rep=='N')
		 {
		  size=i+1;
		  break;
		  }
		  else
		  size=i+1;
		  }}
//-------------------------------------------------------



void to_do_list::tdisplaydata()
{
	 clrscr();
	 systime();
	 sysdate();
	 cout<<"\n\n";
	 line(78,'+');
	 cout<<"  DATE "<<" (Day/Month/Year): ";
	 printf("%1d/%1d/%1d",d1.dd,d1.mm,d1.yy);
	 cout<<"\n";
	 line(78,'+');
	 cout<<"\n\n";
	 for(int i=0;i<size;i++)
	 {
		gotoxy(4,i+11);
		cout<<i+1<<"  :  ";
		gotoxy(10,i+11);
		puts(job_to_do[i]);
		}
		}
//-------------------------------------------------------------
void tdadd()
{
	fstream file;
	file.open("to_do.dat",ios::app|ios::binary);
	to_do_list td;
	char rep;
	do
	{
		 td.tgetdata();
		 file.write((char*)&td,sizeof(td));
		 cout<<"\n\n\t Want to add more records (y/n): ";
		 cin>>rep;
		 }while(rep=='y'||rep=='Y');
		}
//------------------------------------------------------------
void tddelete()
{
	 Date2 d;
	 clrscr();
	 systime();
	 sysdate();
	 char rep;
	 fstream fin,fout;
	 fin.open("to_do.dat",ios::in|ios::binary);
	 fout.open("Nto_do.dat",ios::out|ios::binary);
	 to_do_list td;
	 cout<<"\n";
	 line(78,'+');
	 cout<<"\n Enter the date of that task which you want to delete : "
	     <<"\nt Day = ";
       cin>>d.dd;
       cout<<"\t Month =";
       cin>>d.mm;
       cout<<"\t Year = ";
       cin>>d.yy;
       cout<<"\n";
       line(78,'+');
       while(fin.read((char*)&td,sizeof(td)))
       {
       if(td.check_dt(d)==1)
       {
	  clrscr();
	  td.tdisplaydata();
	  getch();
       cout<<"\n\t Is this the Task you want to delete: ";
	  cin>>rep;
	  if(rep=='y'|| rep=='Y')
	  {
		clrscr();
		systime();
		sysdate();
		line(78,'*');
		cout<<"\t\t  DELETED!! \n";
		line(78,'*');
		}

	  else
	  {
		cout<<"\n\t\t  No Problem  ";
		fout.write((char*)&td,sizeof(td));
		}
		}
		else
		fout.write((char*)&td,sizeof(td));
		fin.close();
		fout.close();
		remove("to_do.dat");
		rename("Nto_do.dat","to_do.dat");
		getch();
		}
	 }
void tdmodify()
{
	int flag;
	clrscr();
	systime();
	sysdate();
	fstream fin,fout;
	fin.open("to_do.dat",ios::in|ios::binary);
	fout.open("Nto_do.dat",ios::out|ios::binary);
	to_do_list td;
	Date2 d;
	cout<<"\n\n";
	line(78,'*');
	 cout<<"Enter the date of that TASK which you want Modify:"
       <<"\nt Day = ";
       cin>>d.dd;
       cout<<"\t Month =";
       cin>>d.mm;
       cout<<"\t Year = ";
       cin>>d.yy;
       cout<<"\n";
       line(78,'+');
       while(fin.read((char*)&td,sizeof(td)))
       {
       if(td.check_dt(d)==1)
       {
	   flag=1;
	   }
       else
       {
       fout.write((char*)&td,sizeof(td));
       }
       }
       if(flag==1)
       {
	clrscr();
	gotoxy(10,5);
	cout<<"Enter the data in Task to be modified \n\n:";
	td.tgetdata();
	fout.write((char*)&td,sizeof(td));
	cout<<"\n\n\t\t     Your Record has been modified!! ";
	}
	if(flag==0)
	{
	clrscr();
	systime();
	sysdate();
	cout<<"\n\n\n\n\n\n\n\n";
	line(78,'*');
	cout<<"\t\t\t No such TASK exist \n";
	line(78,'*');
	}
	fin.close();
	fout.close();
	remove("to_do.dat");
	rename("Nto_do.dat","to_do.dat");
	getch();
	}
	//------------------------------------------------------------------
void tdsview()
{
	to_do_list td;
	clrscr();
	systime();
	sysdate();
	Date2 d;
	int flag=0;
	ifstream fin("to_do.dat",ios::in | ios::binary);
	cout<<"\n\n";
	line(78,'+');
	cout<<" Enter the date of that Task which you want to delete"
	    <<"\nt Day = ";
       cin>>d.dd;
       cout<<"\t Month =";
       cin>>d.mm;
       cout<<"\t Year = ";
       cin>>d.yy;
       cout<<"\n";
       line(78,'+');
       while(fin.read((char*)&td,sizeof(td)))
       {
       if(td.check_dt(d)==1)
       {
	  clrscr();
	  td.tdisplaydata();
	  flag=1;
	  }}
	  if(flag!=1)
	  {
	  clrscr();
	  systime();
	  sysdate();
	  cout<<"\n\n\n\n\n\n\n\n";
	  line(78,'*');
	  cout<<"\t\t\t No such Task exist \n";
	  line(78,'*');
	 }
	 getch();
	 fin.close();
 }
//-------------------------------CLASS HAND (CLOCK)----------------
   //set path for graphics files are located here
   #define BGI_PATH "C:\Turboc4\TC\BGI"
	void systime();
	void sysdate();
	const double PI=3.14159265;
	const double DEGS_PER_SEC=6.0;
	const double DEGS_PER_MIN=6.0;
	const double DEGS_PER_HOUR=30.0;
	const double SEC_PER_MIN=60.0;
	const double MIN_PER_HOUR=60.0;
	int maxx,maxy;
	void Initialize(void);
	void ShowFrame(void);
inline double ToRadians(double theta)
{
	return (theta*PI)/180.0;
	}
class hand
{
	 int length ,color;
	 double angle,prev_angle;
	 public:
	 hand(int len,int clr)
	 {
		length=len;
		 color=clr;
		 prev_angle=angle=0;
	 }
	 void SetAngle(double theta)
	 {
		 angle=theta;
	 }
	 void Copy()
	 {
		 prev_angle=angle;
	 }
	 int Change()
	 {
		 return (angle!=prev_angle);
	 }
	 void display(int x,int y) ;
	 void erase(int x,int y);
};
void hand::display(int x,int y)
{
	int x1,y1;
	x1=x+length*sin(angle);
	y1=y-length*cos(angle);
	setcolor(color);
	setfillstyle( EMPTY_FILL,color);
	line(x,y,x1,y1);
	}



// Erase previously drawn hand
void hand::erase( int x, int y )
{
	int x1, y1;
	x1=x+length*sin(prev_angle);

	y1=y-length*cos(prev_angle);//y increases from top to bottom
	setcolor(BLACK);//Backgnd color
	 line (x,y,x1,y1);
}

int clocka()
{
	systime();
	sysdate();
	struct tm* ptr_curtime; // Pointer to current time
	time_t curtime;
	//Holds current time
	hand hour( 55, WHITE ), minute( 85, WHITE ), second( 70, BROWN );
	int h_angle, m_angle, s_angle;
	int center_x, center_y  ;
	// Clock center
	Initialize();
	ShowFrame();
	center_x = (maxx+1)/2;
	center_y=(maxy+1)/2;

while(!kbhit())
{
	hour.Copy(); //Store previous values
	minute.Copy();
	second.Copy();
	//Get current time
	curtime = time(&curtime);
	ptr_curtime= localtime (&curtime);
	// Calculate the angles of hands
h_angle = ( ptr_curtime->tm_hour%12 * DEGS_PER_HOUR ) +(ptr_curtime->tm_min * ( DEGS_PER_HOUR / MIN_PER_HOUR));

	// Ignore change in seconds
m_angle = ( ptr_curtime->tm_min * DEGS_PER_MIN) + ( ptr_curtime->tm_sec * (DEGS_PER_MIN / SEC_PER_MIN )) ;
	//Minute hand  position also changes with secs
s_angle = ptr_curtime->tm_sec * DEGS_PER_SEC,

hour.SetAngle(ToRadians(h_angle));
minute.SetAngle(ToRadians(m_angle));
second.SetAngle(ToRadians(s_angle));
// Display all hands if second position has changed
if(second.Change())
{
	second.erase( center_x,center_y);
	second.display( center_x, center_y);
	minute.erase(center_x, center_y) ;
	minute.display(center_x, center_y);
	hour.erase(center_x, center_y );
	hour.display( center_x, center_y );
}
}
//End of while(!kbhit())
closegraph();
restorecrtmode();
getch();

return 0;
}

void Initialize( void )
{
	int GraphDriver=DETECT, GraphMode;
	int ErrorCode;
	initgraph(&GraphDriver,&GraphMode, BGI_PATH );
	ErrorCode = graphresult();
	if(ErrorCode != grOk )
{
	cout<<"\nGraphics system error: "<<grapherrormsg( ErrorCode );
	exit(1);
}
	maxx = getmaxx();
	maxy = getmaxy();
}
// Shows the clock frame
void ShowFrame(void)
{
	systime();
	sysdate();
	setcolor(WHITE);
	int x1, y1;
	circle( (maxx+1)/2, (maxy+1)/2, 130 );
	circle( (maxx+1)/2, (maxy+1)/2, 133 );
	for(int angle= 0; angle < 360; angle+=15)
	{
		x1 = (maxx+1)/2 + 95*cos(ToRadians(angle));
	y1 = (maxy+1)/2 - 95*sin(ToRadians (angle));
	putpixel(x1,y1,WHITE);

	}
outtextxy (430,236,"3");
outtextxy(415,185,"2");
outtextxy(375,144,"1");
outtextxy (313,130,"12");
outtextxy(254, 144,"11");
outtextxy (218,181,"10");
outtextxy (210, 235,"9");
outtextxy(224, 288,"8");
outtextxy(264,335,"7");
outtextxy(319,349,"6");
outtextxy(374,335,"5");
outtextxy(415, 292,"4");

}
//-------------------------------CALCULATOR---------------------------
char pexp[1000],op1[2];
//-----------------------------------------------------------------------
void calculator()  // function to take the input, Invoke convert function &display answer
{
	come_here: //label to repeat the program
	clrscr();
	systime();
	sysdate();
	cout<<"\n";
	line(78,'*');
	cout<<"\t\t\t      Calculator \n";
	line(78,'*');
	gotoxy(17,11);
	textattr(WHITE+BLINK);
	cprintf("    Press any key to start");
	getch();
	textcolor(15);
	inp_convert();  //inputs the infix expression

	evaluate(pexp);  //Evaluate the infix expression & displays the result
	textattr(WHITE);
	cout<<"\n\n\n\t\t     "<<"Continue??(y/n)  :";

	gets(op1);
	if(strcmp (op1,"y")==0)
	{
	goto come_here;
	}
	else if (strcmp(op1,"n")==0)
	{
	menu();
	}
	}
  //---------------------------ClASS ISTACK-------------------------//

 extern char pexp[1000];// this contains the input string

//-----------------------------------------------------------------

class istack
{
	int top;
	public:
	char Stack[100],Iexp[100], Pexp[100];
	istack()// constructor to assign  initial null  value to variables
	{
	 top=-1;
	Stack[0]=NULL;
	Iexp[0]=NULL;
	Pexp[0]=NULL;
	}

void getdata() // function to take the input expression from user
{
	 cout<<"\n\n";
	line(78,'-');
	cout<<"\t\t Enter the Expression : ";
	gets(Iexp);
	line(78,'-');
	cout<<"\n\n";
	}
	void push(char); // function to push data in the stack
	char pop(); // function to remove data from the stack

	void infix_to_postfix();  // function to convert input infix expression to postfix expression

};

//-----------------------------------------------------------------

void inp_convert() // function to invoke infix to postfix function
{
	clrscr();
	systime();
	sysdate();
	istack s;
	s.getdata();
	s.infix_to_postfix();
	getch();
	strcpy(pexp,s.Pexp);
	cout<<pexp<<endl;
}
//-------------------------------------------------------------------

void istack:: push(char Symbol)
{
	if(top==99)
	cout<<"Error :Stack is full."<<endl;
else
{
	top++;
	Stack[top]=Symbol;
}
}
/***************************************************************************///--------------------pop()-------------------------------------------------------///*********************************/constchar pop()
char istack:: pop()
{
	char Symbol=NULL;

	if (top==-1)
	cout<<"Error : Stack is empty ."<<endl;

	else
	{
	Symbol=Stack[top];
	Stack[top]=NULL;
	top--;
}

return Symbol;
}
/****************************************************************************///---------------------------------------------infix_to_postfix(const char *)----------------------------////*****************************************************************/void infix_to_postfix(constchar
void istack::infix_to_postfix()
{
	char Symbol[5]={NULL};
	char Temp[5]={NULL};
	int f=0;
	for(int count=0;count<strlen(Iexp);count++)
	{
	Symbol [0]=Iexp[count ] ;
	if(!isalnum(Iexp[count])&&f==0)
	{
	strcat(Pexp," ");
	f=1;
	}
else
	f=0;
	if(Symbol[0]=='(')
	push(Symbol[0]);
else if(Symbol[0]=='(')
{
	Symbol[0]=pop();
	while (Symbol[0]!='(')
{
	strcat (Pexp, Symbol);
	Symbol[0]=pop();
}
	f=0;
}
else if(Symbol[0]=='^'|| Symbol[0]=='*'|| Symbol[0]=='/'|| Symbol [0]=='+' || Symbol[0]=='-')
{
	if((Symbol [0]=='*'|| Symbol [0]=='/') && top !=-1)
	{
		Temp[0]=pop();
		while( (Temp[0]=='^'||Temp[0]=='*'||Temp[0]=='/') && top !=-1)
		{
			strcat (Pexp, Temp);
			Temp[0]=pop();
			}
		push(Temp[0]);
}

else if( (Symbol [0]=='+'||Symbol [0]=='-')&& top !=-1)
{
	Temp[0]=pop();
while(Temp[0]!='('|| top!=-1)
{
	strcat (Pexp, Temp);
	Temp[0]=pop();
}
	push(Temp[0]);
}
	push(Symbol[0]);
}
else
{
	strcat (Pexp,Symbol);
}
}
while(top!=-1)
{
	Temp[0]=pop();
	strcat(Pexp," ");
	strcat(Pexp, Temp);
}

}

//-------------------------------CLASS STACK-------------------------//
 //-----------------------------------------------------------------

class stack
{
	long double s[50];
	int top;
	public:
	stack()
	{
	top=-1;
	}
	void push(long double); // function to push data in the stack
	long pop(); // function to remove data from the stack

};
//-------------------------------------------------------------------------------------------------------
void stack:: push (long double l) //function to push operators into a stack
{
	if (top+1==50)
	cout<<"\n Overflow";
	else
	{
	s[++top]=l;
	}
	}
//------------------------------------------------------------------
long stack::pop() //function to pop
{
	if (top==-1)
	return -1;
	else
	{
	long v=s[top--];
	return v; // contains the value which has been popped
	}
}
//-----------------------------------------------------------------

long int isOperator(char ch) // functon to check the operator
{
	if (ch=='+' || ch=='*' || ch=='*' || ch=='/')
	return 1;
	else
	return 0;
}
//--------------------------------------------------------------------------------------------------------------------------------------------------

//function to perform the various operations such as addition, subtraction,etc.

long double performOperation(long double op1, long double op2, char op)
{
	long double ans;
	switch (op)
	{
	case'+' :  ans= op2 + op1;
		   break;
	case'-'  : ans = op2 -op1;
		   break;
	case'*'  : ans=op2*op1;
		   break;
	case'/' :  ans = op2 / op1;
		   break;
	}
return ans;
}
//-------------------------------------------------------------------------------------------------------------------------------------

int evaluate(char pexp[100]) //function to evaluate the given expression
{
	stack s;
	char exp[100], buffer[15];
	int i,j,len;
	long double op1, op2, x;
	strcpy (exp,pexp);
	len=strlen(exp);
	j=0;
	for (i=0; i<len;i++)
	{
	if (exp[i]>='0' && exp [i]<='9'){
	buffer[j++] =exp [i];
	}
	else if(exp[i]==' ' ){
	if (j>0){
	buffer[j]='\0';

	x = atol(buffer);
	s.push(x);
	j=0;
	}
}
	else if (isOperator(exp[i]))
{
	op1=s.pop();
	op2=s.pop();
	s.push(performOperation(op1, op2, exp[i]));
	}
}
	line(78,'-');
	printf("\t\t\t\t Answer= %d\n" , s.pop());
	line (78,'-');
	getch();
	return 0;
}
//-----------------------------Thank YOU---------------------------------------

void thank() //function that creates the thank you screen
{
	clrscr();
	int x1=41, y1=2;
	for(int i=0; i<21; i++)
	{
	int g=random(16)+1;
	if (i>=0 && i<5)
	{
	x1+=4;
	y1+=2;
	}
	if (i>=5 && i<10)
	{
	x1-=4;
	y1+=2;
	}
	if (i>=10 &&i<15)
	{
	x1-=4;
	y1-=2;
	}
	if (i>=15 && i<20)
	{
	x1+=4;
	y1-=2;
	}

	gotoxy (x1,y1);
	textcolor(g);
	cprintf("\t");
	sound((i-1)*30);
	delay (100);
}
	gotoxy (36,12);
	cout<<" ";
	delay (50);
	cout<<"T";
	delay (50);
	cout<< "H";
	delay (50);
	cout<<"A";
	delay(50);
	cout<<"N";
	delay (50);
	cout<<"K";
	delay(50);

	cout<<" ";
	delay(50);
	cout<<"Y";
	delay(50);
	cout<<"O";
	delay(50);
	cout<<"U";
	delay (50);
	cout<<"!";
	delay(50);
	cout<<"";
	delay(50);
	cout<<" ";
	delay(50);
	gotoxy (34, wherey()+1);
	for (i=0;i<17;i++)
	{
	cout<<"-";
	sound((i+1)*100);
	delay(50);
	}
	gotoxy (wherex()-1,wherey()-1);
	cout<<"|";
	gotoxy (wherex()+1,wherey()-1);
	int x2=wherex()-2;
	for (i=0;i<17;i++)
	{
	 gotoxy (x2,wherey());
	cout<<"-";
	x2--;
	delay(50);
	}
	gotoxy (wherex()-1, wherey()+1);
	cout<<"|";
	gotoxy (1,23);
	nosound() ;
	textcolor(WHITE+BLINK);
	cprintf( "PRESS ANY KEY TO END");
	gotoxy (58,23);
	textcolor(WHITE+BLINK);
	cprintf(" PRESS ANY KEY TO END");
	getch();
	textattr(WHITE);
}
//-------------------------------CLASS DIGITE-------------------------
class DIGITE
{
	private:
	int X,Y;
	int SIZE;
	int COLOR;
	int HIGHT, WIDTH;
	void *VER,*HOR;
	public:
	int style;
	DIGITE();
	DIGITE(int s, int c, int st);
	void MakeBars();
	void PutDig(int x, int y, int n);
	void DrawDig (int x, int y, int n);
	~DIGITE();
	};
//--------------------------FUNCTIONS FOR DIGITE---------------------
DIGITE:: DIGITE()
{
	style= 1;
	SIZE=1;
	HIGHT=1;
	WIDTH =12;
	COLOR=15;
	MakeBars();
	}

DIGITE:: DIGITE (int s, int c , int st)
{

	style=st;
	SIZE=s;
	COLOR=15;
	HIGHT=1;
	WIDTH=6;
	MakeBars();
	cout<<c;
	}
void DIGITE::MakeBars()
{
	int x,y,w,h;
	int area;
	x=10;
	y=getmaxy()/2;
	w=WIDTH*SIZE;
	h= HIGHT*SIZE;
	setfillstyle(style,COLOR);
	bar(x+h,y,x+w-h,y+h);
	setcolor(COLOR);
	line(x,y+h/2,x+h,y);
	line(x,y+h/2,x+h,y+h);
	floodfill(x+h/2,y+h/2,COLOR);
	setcolor(COLOR);
	line(x+w-h, y, x+w+-h+h,y+h/2);
	line(x+w-h, y+h, x+w-h+h,y+h/2);
	floodfill(x+w-h+h/2,y+h/2, COLOR);
	area=imagesize(x,y,x+w,y+h);
	HOR=malloc(area);
	if(HOR==NULL)
	{
	printf("Low Memory (Horizontal)... \nPress Any Key");
	getch();
	exit(0);
	}
	getimage(x,y,x+w,y+h, HOR);
	putimage(x, y,HOR, XOR_PUT);
	x=getmaxx()/2;
	y=10;
	h=SIZE* WIDTH;
	w=HIGHT* SIZE;
	bar(x, y+w,x+w,y+h-w);
	setcolor(COLOR);
	line(x,y+w, x+w/2,y);
	line(x+w,y+w,x+w/2,y);
	floodfill(x+w/2,y+w/2,COLOR);
	setcolor(COLOR);
	line(x,y+h-w,x+w/2,y+h);
	line(x+w,y+h-w, x+w/2,y+h);
	floodfill(x+w/2,y+h-w/2,COLOR);
	area=imagesize(x,y,x+w,y+h);
	VER= malloc(area);
	if(VER==NULL)
	{
	printf("Low Memory(Vertical)... \nPress Any Key");
	getch();
	exit(0);}
	getimage(x, y, x+w,y+h, VER);
	putimage(x, y, VER, XOR_PUT);
}

void DIGITE::PutDig(int x, int y, int n)
{
	int num, gap, qu, rem;
	gap=(HIGHT*SIZE);

	num=n;
	if(num>=0 && num<10)
	{
	DrawDig(x,y,0);
	DrawDig(x+WIDTH*SIZE+HIGHT*SIZE+gap,y,n);
	}
	if(num>=10 && num<100)
	{
	qu=num/10;
	rem=num%10;
	DrawDig(x,y,qu);
	DrawDig(x+WIDTH* SIZE+HIGHT* SIZE+gap,y,rem);
	}
}
void DIGITE::DrawDig(int x, int y, int n)
{
	int H[10][3]={  1,0,1,
			0,0,0,
			1,1,1,
			1,1,1,
			0,1,0,
			1,1,1,
			1,1,1,
			1,0,0,
			1,1,1,
			1,1,1
			};
	int V[10][4]={  1,1,1,1,
			0,1,0,1,
			0,1,1,0,
			0,1,0,1,
			1,1,0,1,
			1,0,0,1,
			1,0,1,1,
			0,1,0,1,
			1,1,1,1,
			1,1,0,1
			};

	int HX[3]={x,x,x};
	int HY[3]={y,y+WIDTH*SIZE, y+2*WIDTH*SIZE};
	int VX[4]={x-(HIGHT*SIZE)/2, x+WIDTH*SIZE-(HIGHT* SIZE)/2, x-(HIGHT* SIZE)/2,x+WIDTH*SIZE- (HIGHT *SIZE)/2};
	int VY[4]={y+(HIGHT *SIZE)/2,y+(HIGHT*SIZE)/2, y+WIDTH *SIZE+(HIGHT*SIZE)/2,y+WIDTH* SIZE+(HIGHT*SIZE)/2};
	int i;
	setfillstyle(1, getpixel(x-1,y-1));

	bar(x-(SIZE *HIGHT)/2,y,x+WIDTH *SIZE+(SIZE* HIGHT)/2, y+2* SIZE*WIDTH+HIGHT*SIZE);
	for(i=0;i<3; i++)
	{
	if(H[n][i]==1)
	putimage(HX[i], HY[i] , HOR, XOR_PUT);
	}
	for(i=0;i<4;i++)

	{
	if(V[n][i])
	putimage(VX[i],VY[i],VER,XOR_PUT);
	}
}

DIGITE::~DIGITE()
{
	delete VER;
	delete HOR;
}
	void drawdot();
	void drawdot2();
	int color=DOTCOLOR;

void clockb()
{
	int gd= DETECT, gm, i;
	initgraph(&gd,&gm,"");
	systime();
	sysdate();
	struct time t;
	int th=t.ti_hour;
	int tm=t.ti_min;
	int ts=t.ti_sec;
	int tms=t.ti_hund;
	//setbkcolor(15);
	gettime(&t);
	DIGITE h(8,DIGCOLOR,1);
	DIGITE m(8,DIGCOLOR,1);
	DIGITE s(8,DIGCOLOR,1);
	DIGITE ms(2,DIGCOLOR, 1);
	int y=200;
	h.PutDig (40,y,t.ti_hour);
	m.PutDig (240,y,t.ti_min);
	s.PutDig(420,y,t.ti_sec);
	ms.PutDig(540,y,t.ti_hund);
	while(!kbhit())
	{
	gettime(&t);
	if(th!=t.ti_hour)
	{
	th=t.ti_hour;
	h.PutDig(40,y,t.ti_hour);
	}
	if(tm !=t.ti_min)
	{
	tm=t.ti_min;
	m.PutDig(240,y,t.ti_min);
	}
	if(ts!=t.ti_sec)
	{
	ts=t.ti_sec;
	s.PutDig(420,y,t.ti_sec);
	drawdot();
	s.PutDig(420,y,t.ti_sec);
	drawdot2();
	color=color-DOTCOLOR;
	if(color<0)
	color= DOTCOLOR;
	}
	if(tms!=t.ti_hund)
	{
	tms=t.ti_hund;
	ms.PutDig(540,y,t.ti_hund);
}
}
	closegraph();
	restorecrtmode();
	getch();
}

void drawdot()
{
	int x,y,gap, size;
	gap=10;
	size=15;
	x=180;
	y=200;
	setfillstyle(1,color);
	bar(x+gap,y +gap,x+gap+size, y +gap+size);
	bar(x+gap,y +8* gap, x+gap+size,y+8*gap+size);

	//color=15;
	delay (0);
	}

void drawdot2()
{
	int x,y,gap, size;
	gap=10;
	size=15;
	x=380;
	y=200;
	setfillstyle(1, color);
	bar(x+gap, y +gap, x+gap+size, y +gap+size);
	bar(x+gap, y +8* gap,x+gap +size, y+ 8 *gap+size);
	//color=15;
	delay(0);
	}


//--------------------------LOAD FUNCTION--------------------------
void load()  //function for creating the loading screen
{
	 clrscr();
	 randomize();
	 int x1=41,y1=2;
	 for (int i=0;i<21;i++)
	 {
	    int g=random(13)+1;
	    if(i>=0 && i<5)
	    {
	      x1+=4;
	      y1+=2;
	    }
	    if(i>=5 && i<10)
	    {
	      x1-=4;
	      y1+=2;
	    }
	    if(i>=10 && i<15)
	    {
	      x1-=4;
	      y1-=2;
	    }
	    if(i>=15 && i<20)
	    {
	      x1+=4;
	      y1-=2;
	    }

	    gotoxy(x1,y1);
	    textcolor(g);
	    cprintf("\t");
	    sound((i+1)*40);
	    delay(100);
	    }
	    gotoxy(36,12);
	    cout<<"D";
	    delay(50);
	    cout<<"i";
	    delay(50);
	    cout<<"g";
	    delay(50);
	    cout<<"i";
	    delay(50);
	    cout<<"t";
	    delay(50);
	    cout<<"a";
	    delay(50);
	    cout<<"l";
	    delay(50);
	    cout<<" ";
	    delay(50);
	    cout<<"D";
	    delay(50);
	    cout<<"i";
	    delay(50);
	    cout<<"a";
	    delay(50);
	    cout<<"r";
	    delay(50);
	    cout<<"y";
	    delay(50);
	    gotoxy(34,wherey()+1);
	    for(i=0;i<17;i++)
	    {
	      cout<<"-";
	      sound((i+1)*100);
	      delay(50);
	    }
	    gotoxy(wherex()-1,wherey()-1);
	    cout<<"|";
	    gotoxy(wherex()+1,wherey()-1);
	    int x2=wherex()-2;
	    for(i=0;i<17;i++)
	    {  gotoxy(x2,wherey());
	      cout<<"-";
	      x2--;
	      delay(50);
	    }
	    gotoxy(wherex()-1,wherey()+1);
	    cout<<"|";
	    gotoxy(1,23);
	    nosound();
	    textcolor(WHITE+BLINK);
	    cprintf("PRESS ANY KEY TO START");
	    gotoxy(58,23);
	    textcolor(WHITE+ BLINK);
	    cprintf("PRESS ANY KEY TO START");
	    getch();
	    textattr(WHITE);
	    clrscr();
	    systime();
	    sysdate();
	    gotoxy(1,10);
	    line(78,'-');
	    gotoxy(1,12);
	    line(78,'-');
	    gotoxy(25,11);
	    cout<<"INITIALIZING WAIT";
	    for (int z=1;z<5;z++)
	    {
	      gotoxy(46,11);
	     {
	       for(int f=0;f<5;f++)
	       {
		 cprintf("\t");
		 delay(300);
		 {
		   gotoxy(46+f,11);
		   cout<<"\b";
		 }
	       }
	     }
	    }
	    textattr(WHITE);
	   }
//----------------------------------------------------------------------


 //                      PASSWORD-
 class passkept
 {
 public:
 char pass[20];
 char* passreturn()
 {
   return pass;
   }
   };

/*______________________function definitions_________________________
       function for main menu and invoking the arrowmenu
       and on the basis of choice, invoke various functions*/
void menu()
{
gotoxy(1,1);
int op;
char op1[2];
systime();
sysdate();
do
{
   op=arrowmenu();
   switch(op)
   {
      case 1 : clrscr();
	       submenu_c();
	       break;
      case 2 : clrscr();
	       submenu_q();
	       break;
      case 3 : clrscr();
	       submenu_sch();
	       break;
      case 4 : clrscr();
	       submenu_todo();
	       break;
      case 5 : clrscr();
	       clockmenu();
	       break;
      case 6 : clrscr();
	       calculator();
	       break;
      case 7 : exit_fun();
	       break;
      default : clrscr();
		systime();
		sysdate();
		cout<<"\n\n\n\n\n\n\n\n";
		line(78,'*');
		cout<<"\t\t\tplease choose a correct option\n";
		line(78,'*');
		cout<<"\n\n\n\t\t     "<<"To continue(y/n)    :  ";
		gets(op1);
		if(strcmp(op1,"y")==0)
		{
		menu();
		}
		else if (strcmp(op1,"n")==0)
		{
		exit(0);
		}
	    }
	    }while(op!=7);
	    }

   //-----------function for exiting the program---------------
void exit_fun()
{
	  char rep[3];
	  clrscr();
	  exitlabel:
	     clrscr();
	     systime();
	     sysdate();
	     cout<<"\n\n\n\t    "<<"Do you want to exit the program ?(y/n)  :";
	     gets(rep);
	     if(strcmpi(rep,"y")==0)
	     {
	       thank();
	       exit(0);
	      }
	    else if(strcmpi(rep,"n")==0)
	    {
	    clrscr();
	    systime();
	    sysdate();
	    cout<<"\n\n\n\n";
	    line(78,'*');
	    cout<<"\t\t\t   returning to main menu !! \n" ;
	    line(78,'*');
	    getch();
	    menu();
	    }
	   else
	   {
	   cout<<"\n\t\t   you entered the wrong choice!!!  ";
	   cout<<"\n\t\t   please try again !!!";
	   getch();
	   goto exitlabel;
	   }
     }
   //--------------------function for creating the main menu--------
int arrowmenu()
{
   clrscr();
   int a,op,x1=26,y1=11;
   textattr(WHITE);
   systime();
   sysdate();
     cout<<"\n\n";
     line(78,'+');
     cout<<"\t\t\t        MAIN MENU\n";
     line(78,'+');
     do
     {
	gotoxy(29,11);
	if(y1==11)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("1.Contacts");
	 gotoxy(29,12);
	if(y1==12)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("2.Query");

	gotoxy(29,13);
	  if(y1==13)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("3.SCHEDULER");
		gotoxy(29,11);
	gotoxy(29,14);
	if(y1==14)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("4.TO DO LIST");
	gotoxy(29,15);
	  if(y1==15)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("5.CLOCK");
	gotoxy(29,16);
	if(y1==16)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("6.CALCULATOR");
	gotoxy(29,17);
	  if(y1==17)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("7.EXIT");
	gotoxy(x1,y1);
	  a=getch();
	  if(a==72)
	  {
	    y1=wherey()-1;
	   }
	  else if(a==80)
	  {
	   y1=wherey()+1;
	   }
	 x1=wherex();
	  if(y1<11)
	     y1=17;
	  if(y1>17)
	     y1=11;
	  cout<<"\b\b    ";
	  gotoxy(x1,y1);
	  cout<<"->";
	}while (a!=13);
	  op=y1-10;
	textattr(WHITE);
	return op;
	}
	       /* function for creating password menu
		   and checking password taken from user */
void passw()
{
     passkept p;
     char cha;
     int i,m,j,z,times=0,count=0;
     char passkept[20]="Manish";
     password:
     clrscr();
     systime();
     sysdate();
     i=0;
     cout<<"\n\n\n";
     line(78,'-');
     gotoxy(1,9);
     line(78,'-');
     gotoxy(1,8);
     cout<<"\t\t    ENTER PASSWORD : ";
     for(int k=0;k<1;k++)
     {
      sound(840);
      delay(200);
      sound(340);
      delay(200);
      sound(440);
      delay(200);
      nosound();
      }
    do
    {
    count=0;
    back:
    cha=getch();
    if(cha==13)
    {
     m=i;
     break;
     }
    else if(cha==8)
    {  count++;
       if(i>0)
       {
       if(i>count)
       {
	 gotoxy(wherex(),wherey());
	 cout<<"\b ";
       }
    else
    {
     gotoxy(wherex()-1,wherey());
     cout<<"\b";
     }
    i--;
    }
    goto back;
    }
    p.pass[i]=cha;
    i++;
    cout<<"*";
    }while(1);
    p.pass[m]='\0';
    if(strcmpi(p.passreturn(),passkept)==0)
    {
    menu();
    }
    else
    {
    times++;
    if(times==4)
    exit(0);
    else
    {
    cout<<"\n\n\n\n\n";
    line(78,'*');
    cout<<" ("<<p.pass<<") is not the right password. Try again !! ( "<<4-times<<"out of 4 attempts left)!!!\n";
	line(78,'*');
	getch();
	goto password;
	}
      }
    }
   /*function for the contact sub-menu and invoking the arrowmenu
     andon the basis of choice, invoke the various functions */
void submenu_c()
{
      clrscr();
      char r[3];
      int op1;
      char op2[2];
      do
      {
	clrscr();
	op1=arrowsubmenu_c();
	switch(op1)
	{
	case 1 : clrscr();
		 cadd();
		 break;
	case 2 : clrscr();
		 cdisplay();
		 break;
	case 3 : clrscr();
		 cdelete();
		 break;
	case 4 : clrscr();
		 cmodify();
		 break;
	case 5 : clrscr();
		 exitlabel:
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n\t     "
		     <<"Do you want to exit this menu ?? (y/n) : ";
		 gets(r);
		 if(strcmpi(r,"y")==0)
		 {
		 menu();
		 }
		 else if(strcmpi(r,"n")==0)
		 {
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n" ;
		 line(78,'*');
		 cout<<"\t\t  returning to contact menu !!\n";
		 line(78,'*');
		 submenu_c();
		 }
		 else
		 {
		  cout<<"\n\t\t  you entered a wrong choice!! ";
		  cout<<"\n\t\t  please try again!!!";
		  getch();
		  goto exitlabel;
		  }
		  break;
	default : clrscr();
		  systime();
		  sysdate();
		  cout<<"\n\n\n\n\n\n\n\n";
		  line(78,'*');
		  cout<<"\t\t\tplease choose a correct option\n";
		  line(78,'*');
		  cout<<"\n\n\n\t\t    "<<"to continue (y/n)   : ";
		  gets(op2);
		  if(strcmp(op2,"y")==0)
		  {
		  submenu_c();
		  }
		  else if(strcmp(op2,"n")==0)
		  {
		  exit(0);
		  }
		  getch();
		  }
		  }while(op1!=5);
}
  /*------functions for the scheduler sub-menu & invoking the arrowmenu
     and on the basisof choice invoking various functions------------*/

void submenu_sch()
{
     clrscr();
     char r[3];
     int op2;
     char op3[2];
     do
     {
       clrscr();
	op2=arrowsubmenu_sch();
	switch(op2)
	{
	case 1 : clrscr();
		 sadd();
		 break;
	case 2 : clrscr();
		 sdisplayall();
		 break;
	case 3 : clrscr();
		 ssview();
		 break;
	case 4 : clrscr();
		 smodify();
		 break;
	case 5 : clrscr();
		 sdelete();
		 break;
	case 6 : clrscr();
		 exitlabel:
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n\t     "
		     <<"Do you want to exit this menu ?? (y/n) : ";
		 gets(r);
		 if(strcmpi(r,"y")==0)
		 {
		 menu();
		 }
		 else if(strcmpi(r,"n")==0)
		 {
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n" ;
		 line(78,'*');
		 cout<<"\t\t returning to scheduler menu!!\n ";
		 line(78,'*');
		 submenu_sch();
		 }
		 else
		 {
		  cout<<"\n\t\t  you entered a wrong choice!! ";
		  cout<<"\n\t\t  please try again!!!";
		  getch();
		  goto exitlabel;
		  }
		  break;
	default : clrscr();
		  systime();
		  sysdate();
		  cout<<"\n\n\n\n\n\n\n\n";
		  line(78,'*');
		  cout<<"\t\t\tplease choose a correct option\n";
		  line(78,'*');
		  cout<<"\n\n\n\t\t    "<<"to continue (y/n)   : ";
		  gets(op3);
		  if(strcmp(op3,"y")==0)
		  {
		  submenu_sch();
		  }
		  else if(strcmp(op3,"n")==0)
		  {
		  exit(0);
		  }
		  getch();
		  }
		  }while(op2!=6);
}

   /*-----------function for the to-do sub menu & invoking the arrowmenu
      and on the basis of choice,invoking the various functions-------*/

void submenu_todo()
{
     clrscr();
     char r[3];
     int op3;
     char op4[2];
     do
     {
       clrscr();
	op3=arrowsubmenu_todo();
	switch(op3)
	{
	case 1 : clrscr();
		 tdadd();
		 break;
	case 2 : clrscr();
		 tdsview();
		 break;
	case 3 : clrscr();
		 tdmodify();
		 break;
	case 4 : clrscr();
		 tddelete();
		 break;
	case 5 : clrscr();
		 exitlabel:
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n\t     "
		     <<"Do you want to exit this menu ?? (y/n) : ";
		 gets(r);
		 if(strcmpi(r,"y")==0)
		 {
		 menu();
		 }
		 else if(strcmpi(r,"n")==0)
		 {
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n" ;
		 line(78,'*');
		 cout<<"\t\t returning to to-do list!!\n ";
		 line(78,'*');
		 getch();
		 submenu_todo();
		 }
		 else
		 {
		  cout<<"\n\t\t  you entered a wrong choice!! ";
		  cout<<"\n\t\t  please try again!!!";
		  getch();
		  goto exitlabel;
		  }
		  break;
	default : clrscr();
		  systime();
		  sysdate();
		  cout<<"\n\n\n\n\n\n\n\n";
		  line(78,'*');
		  cout<<"\t\t\tplease choose a correct option\n";
		  line(78,'*');
		  cout<<"\n\n\n\t\t    "<<"to continue (y/n)   : ";
		  gets(op4);
		  if(strcmp(op4,"y")==0)
		  {
		  submenu_todo();
		  }
		  else if(strcmp(op4,"n")==0)
		  {
		  exit(0);
		  }
		  getch();
		  }
		  }while(op3!=5);
}
   /*------functions for the query sub-menu & invoking the arrowmenu
     and on the basisof choice invoking various functions------------*/

void submenu_q()
{
     clrscr();
     char r[3];
     int op4;
     char op5[2];
     do
     {
       clrscr();
	op4=arrowsubmenu_q();
	switch(op4)
	{
	case 1 : clrscr();
		 cphone_disp();
		 break;
	case 2 : clrscr();
		 cname_disp();
		 break;
	case 3 : clrscr();
		 caddress_disp();
		 break;
	case 4 : clrscr();
		 cmobile_disp();
		 break;
	case 5 : clrscr();
		 ccompany_disp();
		 break;
	case 6 : clrscr();
		 exitlabel:
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n\t     "
		     <<"Do you want to exit this menu ?? (y/n) : ";
		 gets(r);
		 if(strcmpi(r,"y")==0)
		 {
		 menu();
		 }
		 else if(strcmpi(r,"n")==0)
		 {
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n" ;
		 line(78,'*');
		 cout<<"\t\t returning to contact query menu!!\n ";
		 line(78,'*');
		 getch();
		 submenu_q();
		 }
		 else
		 {
		  cout<<"\n\t\t  you entered a wrong choice!! ";
		  cout<<"\n\t\t  please try again!!!";
		  getch();
		  goto exitlabel;
		  }
		  break;
	default : clrscr();
		  systime();
		  sysdate();
		  cout<<"\n\n\n\n\n\n\n\n";
		  line(78,'*');
		  cout<<"\t\t\tplease choose a correct option\n";
		  line(78,'*');
		  cout<<"\n\n\n\t\t    "<<"to continue (y/n)   : ";
		  gets(op5);
		  if(strcmp(op5,"y")==0)
		  {
		  submenu_q();
		  }
		  else if(strcmp(op5,"n")==0)
		  {
		  exit(0);
		  }
		  getch();
		  }
		  }while(op4!=6);
		  }
  //--------------------function for creating the query sub menu--------
int arrowsubmenu_q()
{
   clrscr();
   int a,op,x1=26,y1=11;
   systime();
   sysdate();
     cout<<"\n\n";
     line(78,'+');
     cout<<"\t\t\t        CONTACT QUERY MENU\n";
     line(78,'+');
     do
     {
	gotoxy(29,11);
	if(y1==11)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("1.VIEW CONTACTS BY PHONE NUMBER");
	gotoxy(29,12);
	  if(y1==12)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("2.VIEW CONTACT BY NAME");
	gotoxy(29,13);
	 if(y1==13)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("3.VIEW CONTACTS BY ADDRESS");
	gotoxy(29,14);
	  if(y1==14)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("4.VIEW CONTACTS BY MOBILE");
	gotoxy(29,15);
	if(y1==15)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("5.VIEW CONTACTS BY COMPANY");
	gotoxy(29,16);
	  if(y1==16)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("6.EXIT TO MAIN MENU");
	  gotoxy(x1,y1);
	  a=getch();
	  if(a==72)
	  {
	    y1=wherey()-1;
	   }
	  else if(a==80)
	  {
	   y1=wherey()+1;
	   }
	 x1=wherex();
	  if(y1<11)
	     y1=16;
	  if(y1>16)
	     y1=11;
	  cout<<"\b\b    ";
	  gotoxy(x1,y1);
	  cout<<"->";
	}while (a!=13);
	  op=y1-10;
	textattr(WHITE);
	return op;
	}
   //--------------------function for creating theto-do query sub menu--------
int arrowsubmenu_todo()
{
   clrscr();
   int a,op,x1=26,y1=11;
   systime();
   sysdate();
     cout<<"\n\n";
     line(78,'+');
     cout<<"\t\t\t        TO DO LIST\n";
     line(78,'+');
     do
     {
	gotoxy(29,11);
	if(y1==11)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("1.ADD TAAST TO-DO");
	gotoxy(29,12);
	  if(y1==12)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("2.VIEW TASKS TO-DO");
	gotoxy(29,13);
	 if(y1==13)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("3.MODIFY TASK YTO-DPO");
	gotoxy(29,14);
	  if(y1==14)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("4.DELETE TO-DO");
	gotoxy(29,15);
	if(y1==15)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("5.EXIT TO MAIN MENU");
	  gotoxy(x1,y1);
	  a=getch();
	  if(a==72)
	  {
	    y1=wherey()-1;
	   }
	  else if(a==80)
	  {
	   y1=wherey()+1;
	   }
	 x1=wherex();
	  if(y1<11)
	     y1=15;
	  if(y1>15)
	     y1=11;
	  cout<<"\b\b    ";
	  gotoxy(x1,y1);
	  cout<<"->";
	}while (a!=13);
	  op=y1-10;
	textattr(WHITE);
	return op;
	}
  //--------------------function for creating the scheduler sub menu--------
int arrowsubmenu_sch()
   {
   clrscr();
   int a,op,x1=26,y1=11;
   systime();
   sysdate();
     cout<<"\n\n";
     line(78,'+');
     cout<<"\t\t\t        SCHDULER MENU\n";
     line(78,'+');
     do
     {
	gotoxy(29,11);
	if(y1==11)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("1.ADD TO SCHEDULE");
	gotoxy(29,12);
	  if(y1==12)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("2.DISPLAY ALL SCHEDULES");
	gotoxy(29,13);
	 if(y1==13)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("3.VIEW SCHEDULES BY DATE");
	gotoxy(29,14);
	  if(y1==14)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("4.MODIFY SCHEDULE");
	gotoxy(29,15);
	if(y1==15)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("5.DELETE SCHEDULE");
	gotoxy(29,16);
	  if(y1==16)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("6.EXIT TO MAIN MENU");
	  gotoxy(x1,y1);
	  a=getch();
	  if(a==72)
	  {
	    y1=wherey()-1;
	   }
	  else if(a==80)
	  {
	   y1=wherey()+1;
	   }
	 x1=wherex();
	  if(y1<11)
	     y1=16;
	  if(y1>16)
	     y1=11;
	  cout<<"\b\b    ";
	  gotoxy(x1,y1);
	  cout<<"->";
	}while (a!=13);
	  op=y1-10;
	textattr(WHITE);
	return op;
}
 //--------------------function for creating the contact sub menu--------
int arrowsubmenu_c()
{
   clrscr();
   int a,op,x1=26,y1=11;
   systime();
   sysdate();
     cout<<"\n\n";
     line(78,'+');
     cout<<"\t\t\t        CONTACT MENU\n";
     line(78,'+');
     do
     {
	gotoxy(29,11);
	if(y1==11)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("1.ADD CONTACTS");
	gotoxy(29,12);
	  if(y1==12)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("2.DISPLAY ALL CONTACTS");
	gotoxy(29,13);
	 if(y1==13)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("3.DELETE CONTACTS");
	gotoxy(29,14);
	  if(y1==14)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("4.MODIFY CONTACTS");
	gotoxy(29,15);
	if(y1==15)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("5.EXIT TO MAIN MENU");
	  gotoxy(x1,y1);
	  a=getch();
	  if(a==72)
	  {
	    y1=wherey()-1;
	   }
	  else if(a==80)
	  {
	   y1=wherey()+1;
	   }
	 x1=wherex();
	  if(y1<11)
	     y1=15;
	  if(y1>15)
	     y1=11;
	  cout<<"\b\b    ";
	  gotoxy(x1,y1);
	  cout<<"->";
	}while (a!=13);
	  op=y1-10;
	textattr(WHITE);
	return op;
	}
   /*------functions for the cock sub-menu & invoking the arrowmenu
     and on the basisof choice invoking various functions------------*/

void clockmenu()
{
     clrscr();
     gotoxy(1,1);
     char r[3];
     int op;
     char op1[2];
     do
     {
       clrscr();
	op=arrowmenuc1();
	switch(op)
	{
	case 1 : clrscr();
		 clocka();
		 break;
	case 2 : clrscr();
		 clockb();
		 break;
	case 3 : clrscr();
		 exitlabel:
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n\t     "
		     <<"Do you want to exit this menu ?? (y/n) : ";
		 gets(r);
		 if(strcmpi(r,"y")==0)
		 {
		 menu();
		 }
		 else if(strcmpi(r,"n")==0)
		 {
		 clrscr();
		 systime();
		 sysdate();
		 cout<<"\n\n\n\n" ;
		 line(78,'*');
		 cout<<"\t\t returning to clock!!\n ";
		 line(78,'*');
		 getch();
		 clockmenu();
		 }
		 else
		 {
		  cout<<"\n\t\t  you entered a wrong choice!! ";
		  cout<<"\n\t\t  please try again!!!";
		  getch();
		  goto exitlabel;
		  }
		  break;
	default : clrscr();
		  systime();
		  sysdate();
		  cout<<"\n\n\n\n\n\n\n\n";
		  line(78,'*');
		  cout<<"\t\t\tplease choose a correct option\n";
		  line(78,'*');
		  cout<<"\n\n\n\t\t    "<<"to continue (y/n)   : ";
		  gets(op1);
		  if(strcmp(op1,"y")==0)
		  {
		  menu();
		  }
		  else if(strcmp(op1,"n")==0)
		  {
		  exit(0);
		  }
		  getch();
		  }
		  }while(op!=7);
		  }
 //--------------------function for creating the clock sub menu--------
int arrowmenuc1()
{
   clrscr();
   int a,op,x1=26,y1=11;
   systime();
   sysdate();
     cout<<"\n\n";
     line(78,'+');
     cout<<"\t\t\t        CLOCK\n";
     line(78,'+');
     do
     {
	gotoxy(29,11);
	if(y1==11)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("1.ANALOG CLOCK");
	gotoxy(29,12);
	  if(y1==12)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("2.DIGITAL CLOCK");
	gotoxy(29,13);
	 if(y1==13)
	  textattr(WHITE+BLINK);
	else
	  textattr(WHITE);
	  cprintf("3.EXIT TO MAIN MENU");
	  gotoxy(x1,y1);
	  a=getch();
	  if(a==72)
	  {
	    y1=wherey()-1;
	   }
	  else if(a==80)
	  {
	   y1=wherey()+1;
	   }
	 x1=wherex();
	  if(y1<11)
	     y1=13;
	  if(y1>13)
	     y1=11;
	  cout<<"\b\b    ";
	  gotoxy(x1,y1);
	  cout<<"->";
	}while (a!=13);
	  op=y1-10;
	textattr(WHITE);
	return op;
	}

     ///*------------------------------MAIN-------------------------*///

void main()
{
	clrscr();
       	load();
       	passw();
	menu();
	getch();
}

 //----------------------END OF PROGRAM-----------------------------//