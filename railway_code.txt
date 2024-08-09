//importing required packages
import java.io.*;
import java.util.*;
import java.sql.*;
//class start
class railway
{
 public static void main(String args[])
 {
   //variable declaration	 
   int ch,num=1;	
   String name="",pass,con_pass,mob,cdate,seats="";
   boolean result;  
   //start of try block   
   try
   {  
     //connecting with database 
     Class.forName("com.mysql.cj.jdbc.Driver");
	 Connection con=DriverManager.getConnection("jdbc:mysql://localhost:3306/railway","root","");
	 Statement stmt=con.createStatement();
	 //login and register
	 System.out.println("RAILWAY ONLINE RESERVATION");
	 System.out.println("***************************");
	 System.out.println();
	 System.out.println("WELCOME");
	 System.out.println("1.login");
	 System.out.println("2.Register");
	 System.out.println("Enter your option:");
	 Scanner s=new Scanner(System.in);
	 ch=s.nextInt();
	 System.out.println("choice:"+ch);
	 System.out.println();
	 //checking whether the user already logged in or not
	 if(ch==1)
     {
       System.out.println("enter the username:");
       name=s.nextLine();
       name=s.nextLine();
	   System.out.println("enter the password:");
	   pass=s.nextLine();
	   ResultSet rs=stmt.executeQuery("SELECT * FROM USERDETAILS WHERE name=\""+name+"\"and password=\""+pass+"\";");   
       result=rs.next();	 
   	   if(result==true)
	   {
		   System.out.println("WARM WELCOME TO YOU!");
	   }
	   //if the user details does not match with database the user need to do registration
	   else
	   {
		   
		System.out.println("User doesnot exists!kindly do register");
		System.out.println("1.login");
		System.out.println("2.Register");
		System.out.println("Enter your option:");
		ch=s.nextInt();
		if(ch==1)
	    {
		  System.out.println("User doesnot exists!kindly do register");
		  System.out.println("1.login");
		  System.out.println("2.Register");
		  System.out.println("Enter your option:");
		  ch=s.nextInt();	
		}       			
	   }   
    }  
	//Registration process
    if(ch==2)
    {
	 System.out.println("enter the username:");
     name=s.nextLine();
     name=s.nextLine();
     System.out.println("enter your mobile no:");
     mob=s.next();
	 if(mob.length()<10||mob.length()>10)
	 {
       System.out.println("mobile no should contain 10 digits only");
       mob=s.next();  
	 }
     System.out.println("enter your password:");
     pass=s.next();	
	 if(pass.length()>8||pass.length()<8)
     {
       System.out.println("password should contain maximum 8 characters");	
	   pass=s.next();	
     }	   
     System.out.println("confirm the password(re-enter):");
     con_pass=s.next();	
	 
     String insert="INSERT INTO USERDETAILS VALUES(\""+name+"\",\""+mob+"\",\""+pass+"\",\""+con_pass+"\");";
     
     int ins=stmt.executeUpdate(insert);  
	 System.out.println("User registered successfully");
	}
	//welcome message
	System.out.println();
	System.out.println("Welcome");
	
	//displaying train details and location
	System.out.println("KINDLY CHOOSE YOUR LOCATION:");
	System.out.println();
	ResultSet rs=stmt.executeQuery("SELECT *FROM location;");
	while(rs.next())
	{
      int id=rs.getInt("id"); 		
	  String from=rs.getString("start");
	  String to=rs.getString("end");
      System.out.println(id+"."+from+"--"+to);
    }	
	//choosing location
	System.out.println("enter your choice:");
	int locch=s.nextInt();
	System.out.println();
	
	//displaying train details based on location entered 
	System.out.println("TRAIN DETAILS:");
	System.out.println();
	ResultSet rs1=stmt.executeQuery("SELECT TRAIN_ID,NAME,T_ID FROM TRAIN WHERE ID=\""+locch+"\";");
	while(rs1.next())
	{
	 String train_name=rs1.getString("name"); 
	 int train_id=rs1.getInt("train_id");
	 int t_id=rs1.getInt("t_id");
	 System.out.println(t_id+"."+train_id+" "+train_name);
	}
	//choosing train
	System.out.println("enter your choice:");
	int trainch=s.nextInt();
	System.out.println();
	
	//date for travel
	System.out.println("Enter the convinient date for booking:");
    cdate=s.next();
	
	String sea[]=new String[25];
	System.out.println();
	//getting no of seats required
	System.out.println("enter the no of seats:");
    int no=s.nextInt();
	System.out.println();
	
	//class preference from the user( 1st or 2nd class)
	System.out.println("KINDLY SELECT YOUR CLASS");
	System.out.println("1.class-1");
	System.out.println("2.class-2");
	System.out.println("enter your choice:");
	int clasch=s.nextInt();
	
	//displaying seat layout for chennai express(chennai to pune)
	if(locch==1&&trainch==1)	
    {
       System.out.println("SEAT LAYOUT");
	   System.out.println("************");
       ResultSet rs2=stmt.executeQuery("SELECT *FROM LOC1_CHENNAI;");
	   while(rs2.next())
	   {	   
       String clas=rs2.getString("class");
       String col1=rs2.getString("col1");
	   String col2=rs2.getString("col2");
	   String col3=rs2.getString("col3");
	   String col4=rs2.getString("col4");
	   String col5=rs2.getString("col5");
	   
	   
	   System.out.println(clas+"    "+col1+"    "+col2+"    "+col3+"    "+col4+"    "+col5);
	   System.out.println();
	   } 
	   System.out.println("**FU** means *FILLED* or *BOOKED* seats please choose other seats");
	   System.out.println();
	   System.out.println("choose your seats:");		
	   for(int i=0;i<no;i++)
	   {
		sea[i]=s.next();
       }	
	   String sep=",";
	   for(int i=0;i<no;i++)
	   {
         seats+=String.join(sep,sea[i]); 
	   }  
	   //System.out.println(seats);
	   for(int i=0;i<no;i++)
	   {
         String update="UPDATE LOC1_CHENNAI SET COL1='FU' WHERE COL1=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update); 
         String update2="UPDATE LOC1_CHENNAI SET COL2='FU' WHERE COL2=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update2);
         String update3="UPDATE LOC1_CHENNAI SET COL3='FU' WHERE COL3=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update3);
         String update4="UPDATE LOC1_CHENNAI SET COL4='FU' WHERE COL4=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update4);
         String update5="UPDATE LOC1_CHENNAI SET COL5='FU' WHERE COL5=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update5);  		 
       } 
	}	
	//displaying seat layout for mysore express(chennai to pune)
	if(locch==1&&trainch==3)	
    {
       System.out.println("SEAT LAYOUT");
	   System.out.println("************");
       ResultSet rs2=stmt.executeQuery("SELECT *FROM LOC1_MYSORE;");
	   while(rs2.next())
	   {	   
       String clas=rs2.getString("class");
       String col1=rs2.getString("col1");
	   String col2=rs2.getString("col2");
	   String col3=rs2.getString("col3");
	   String col4=rs2.getString("col4");
	   String col5=rs2.getString("col5");
	   
	   
	   System.out.println(clas+"    "+col1+"    "+col2+"    "+col3+"    "+col4+"    "+col5);
	   System.out.println();
	   } 
	   System.out.println("**FU** means *FILLED* or *BOOKED* seats please choose other seats");
	   System.out.println();
	   System.out.println("choose your seats:");		
	   for(int i=0;i<no;i++)
	   {
		sea[i]=s.next();
       }	
	   String sep=",";
	   for(int i=0;i<no;i++)
	   {
         seats+=String.join(sep,sea[i]); 
	   }  
	   //System.out.println(seats);
	   for(int i=0;i<no;i++)
	   {
         String update="UPDATE LOC1_MYSORE SET COL1='FU' WHERE COL1=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update); 
         String update2="UPDATE LOC1_MYSORE SET COL2='FU' WHERE COL2=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update2);
         String update3="UPDATE LOC1_MYSORE SET COL3='FU' WHERE COL3=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update3);
         String update4="UPDATE LOC1_MYSORE SET COL4='FU' WHERE COL4=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update4);
         String update5="UPDATE LOC1_MYSORE SET COL5='FU' WHERE COL5=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update5);  		 
       } 
	}	
	//displaying seat layout for mumbai express(mumbai to hyderabad)
	if(locch==2&&trainch==2)	
    {
       System.out.println("SEAT LAYOUT");
	   System.out.println("************");
       ResultSet rs2=stmt.executeQuery("SELECT *FROM LOC2_MUMBAI;");
	   while(rs2.next())
	   {	   
       String clas=rs2.getString("class");
       String col1=rs2.getString("col1");
	   String col2=rs2.getString("col2");
	   String col3=rs2.getString("col3");
	   String col4=rs2.getString("col4");
	   String col5=rs2.getString("col5");
	   
	   
	   System.out.println(clas+"    "+col1+"    "+col2+"    "+col3+"    "+col4+"    "+col5);
	   System.out.println();
	   } 
	   System.out.println("**FU** means *FILLED* or *BOOKED* seats please choose other seats");
	   System.out.println();
	   System.out.println("choose your seats:");		
	   for(int i=0;i<no;i++)
	   {
		sea[i]=s.next();
       }	
	   String sep=",";
	   for(int i=0;i<no;i++)
	   {
         seats+=String.join(sep,sea[i]); 
	   }  
	   //System.out.println(seats);
	   for(int i=0;i<no;i++)
	   {
         String update="UPDATE LOC2_MUMBAI SET COL1='FU' WHERE COL1=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update); 
         String update2="UPDATE LOC2_MUMBAI SET COL2='FU' WHERE COL2=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update2);
         String update3="UPDATE LOC2_MUMBAI SET COL3='FU' WHERE COL3=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update3);
         String update4="UPDATE LOC2_MUMBAI SET COL4='FU' WHERE COL4=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update4);
         String update5="UPDATE LOC2_MUMBAI SET COL5='FU' WHERE COL5=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update5);  		 
       } 
	}
    //displaying seat layout for mysore express(mumbai to hyderabad) 
    if(locch==2&&trainch==3)	
    {
       System.out.println("SEAT LAYOUT");
	   System.out.println("************");
       ResultSet rs2=stmt.executeQuery("SELECT *FROM LOC2_MYSORE;");
	   while(rs2.next())
	   {	   
       String clas=rs2.getString("class");
       String col1=rs2.getString("col1");
	   String col2=rs2.getString("col2");
	   String col3=rs2.getString("col3");
	   String col4=rs2.getString("col4");
	   String col5=rs2.getString("col5");
	   
	   
	   System.out.println(clas+"    "+col1+"    "+col2+"    "+col3+"    "+col4+"    "+col5);
	   System.out.println();
	   } 
	   System.out.println("**FU** means *FILLED* or *BOOKED* seats please choose other seats");
	   System.out.println();
	   System.out.println("choose your seats:");		
	   for(int i=0;i<no;i++)
	   {
		sea[i]=s.next();
       }	
	   String sep=",";
	   for(int i=0;i<no;i++)
	   {
         seats+=String.join(sep,sea[i]); 
	   }  
	   //System.out.println(seats);
	   for(int i=0;i<no;i++)
	   {
         String update="UPDATE LOC2_MYSORE SET COL1='FU' WHERE COL1=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update); 
         String update2="UPDATE LOC2_MYSORE SET COL2='FU' WHERE COL2=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update2);
         String update3="UPDATE LOC2_MYSORE SET COL3='FU' WHERE COL3=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update3);
         String update4="UPDATE LOC2_MYSORE SET COL4='FU' WHERE COL4=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update4);
         String update5="UPDATE LOC2_MYSORE SET COL5='FU' WHERE COL5=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update5);  		 
       } 
	}	
    //displaying seat layout for mysore express(pune to mumbai) 
    if(locch==3&&trainch==3)	
    {
       System.out.println("SEAT LAYOUT");
	   System.out.println("************");
       ResultSet rs2=stmt.executeQuery("SELECT *FROM LOC3_MYSORE;");
	   while(rs2.next())
	   {	   
       String clas=rs2.getString("class");
       String col1=rs2.getString("col1");
	   String col2=rs2.getString("col2");
	   String col3=rs2.getString("col3");
	   String col4=rs2.getString("col4");
	   String col5=rs2.getString("col5");
	   
	   
	   System.out.println(clas+"    "+col1+"    "+col2+"    "+col3+"    "+col4+"    "+col5);
	   System.out.println();
	   } 
	   System.out.println("**FU** means *FILLED* or *BOOKED* seats please choose other seats");
	   System.out.println();
	   System.out.println("choose your seats:");		
	   for(int i=0;i<no;i++)
	   {
		sea[i]=s.next();
       }	
	   String sep=",";
	   for(int i=0;i<no;i++)
	   {
         seats+=String.join(sep,sea[i]); 
	   }  
	   //System.out.println(seats);
	   for(int i=0;i<no;i++)
	   {
         String update="UPDATE LOC3_MYSORE SET COL1='FU' WHERE COL1=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update); 
         String update2="UPDATE LOC3_MYSORE SET COL2='FU' WHERE COL2=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update2);
         String update3="UPDATE LOC3_MYSORE SET COL3='FU' WHERE COL3=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update3);
         String update4="UPDATE LOC3_MYSORE SET COL4='FU' WHERE COL4=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update4);
         String update5="UPDATE LOC3_MYSORE SET COL5='FU' WHERE COL5=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update5);  		 
       } 
	}	
    //displaying seat layout for sabari express(varanasi to chennai) 
    if(locch==4&&trainch==4)	
    {
       System.out.println("SEAT LAYOUT");
	   System.out.println("************");
       ResultSet rs2=stmt.executeQuery("SELECT *FROM LOC4_SABARI;");
	   while(rs2.next())
	   {	   
       String clas=rs2.getString("class");
       String col1=rs2.getString("col1");
	   String col2=rs2.getString("col2");
	   String col3=rs2.getString("col3");
	   String col4=rs2.getString("col4");
	   String col5=rs2.getString("col5");
	   
	   
	   System.out.println(clas+"    "+col1+"    "+col2+"    "+col3+"    "+col4+"    "+col5);
	   System.out.println();
	   } 
	   System.out.println("**FU** means *FILLED* or *BOOKED* seats please choose other seats");
	   System.out.println();
	   System.out.println("choose your seats:");		
	   for(int i=0;i<no;i++)
	   {
		sea[i]=s.next();
       }	
	   String sep=",";
	   for(int i=0;i<no;i++)
	   {
         seats+=String.join(sep,sea[i]); 
	   }  
	   //System.out.println(seats);
	   for(int i=0;i<no;i++)
	   {
         String update="UPDATE LOC4_SABARI SET COL1='FU' WHERE COL1=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update); 
         String update2="UPDATE LOC4_SABARI SET COL2='FU' WHERE COL2=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update2);
         String update3="UPDATE LOC4_SABARI SET COL3='FU' WHERE COL3=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update3);
         String update4="UPDATE LOC4_SABARI SET COL4='FU' WHERE COL4=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update4);
         String update5="UPDATE LOC4_SABARI SET COL5='FU' WHERE COL5=\""+sea[i]+"\";"; 
         stmt.executeUpdate(update5);  		 
       } 
	}	
   System.out.println();
   //displaying ticket details to user
   System.out.println("TICKET DETAILS");
   System.out.println("***************");
   ResultSet rs3=stmt.executeQuery("SELECT START,END FROM LOCATION WHERE ID=\""+locch+"\";");
   while(rs3.next())
   {
	 String start=rs3.getString("start");
	 String end=rs3.getString("end");
     System.out.println("FROM: "+start+" "+"TO: "+end);
   }
   ResultSet rs4=stmt.executeQuery("SELECT TRAIN_ID,NAME FROM TRAIN WHERE T_ID=\""+trainch+"\";");
   while(rs4.next())
   {
	int tr_id=rs4.getInt("train_id");   
    String name_tr=rs4.getString("name"); 	   
    System.out.println("TRAIN ID AND NAME:"+tr_id+" "+name_tr);
	break;
   } 	
   Calendar cal=Calendar.getInstance();
   String date_time=cal.getTime().toString();
   System.out.println("BOOKED DATE AND TIME:"+date_time);
      
   System.out.println("DATE FOR JOURNEY:"+cdate);
   if(clasch==1)
   {
      int price=250;
	  System.out.println("ONE TICKET PRICE:"+price);
   }	
   else if(clasch==2)
   {
     int price=150;
	 System.out.println("ONE TICKET PRICE:"+price);
   }	
   
   System.out.println("ADMIT:"+no);  
   System.out.print("SEAT NO:");
   System.out.print(seats);	
   System.out.println();
   if(clasch==1)
   {
      int total=no*250;
     System.out.println("TOTAL PRICE:"+total);
   }	
   else if(clasch==2)
   {
     int total=no*150;
     System.out.println("TOTAL PRICE:"+total);
   }	   
   
   System.out.println();
   System.out.println();
   System.out.println("THANK YOU FOR BOOKING TICKETS");
   }//try end
   
   //start of catch block
   catch(Exception ex)
   {
     System.out.println("database not connected");
     System.out.println(ex.toString());
   }//catch end
 } //main method end
} // class end
	