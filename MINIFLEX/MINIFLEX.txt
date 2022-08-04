#include <iostream>
#include <cstdlib>
#include <fstream>
#include <sstream> 

using namespace std;

//convert lowercase to uppercase

string convert(string str)
{
	for (int i=0; i<str.length(); i++)
	{
		str[i]=toupper(str.at(i));
	}
	return str;
}

class Person
{
	public:
		virtual bool check()=0;
		virtual ~Person()
		{}
};

//Student Class
class Student:public Person
{
	protected:
		string username;
		string password;
	public:
		Student(string name, string pass)
		{
			username=name;
			password=pass;	
		}
		void display()
		{
			cout<<"Username: "<<username<<endl;
			cout<<"Passward: "<<password<<endl;
		}
};

//Class to check username and passwards of students
class checkLoginStudent: public Student
{
	public:
		checkLoginStudent(string name, string pass):Student(name, pass)
		{}
		
		bool check()
		{
			bool flage;
			string name;
			string pass;
			int countName=0;
			int countPass=0;
			ifstream infile;
			infile.open("C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\Studentpasswords.txt");
			while(!infile.eof())
			{
				infile>>name;
				infile>>pass;
				flage=false;
				countName=0;
				countPass=0;	
				for (int i=0; i<name.length(); i++)
				{
					if (username.at(i)==name.at(i))
					{
						countName++;
					}
					else 
						break;
				}
				for (int i=0; i<pass.length(); i++)
				{
					if (password.at(i)==pass.at(i))
					{
						countPass++;
					}
					else 
						break;
				}
				if (countName==name.length())
				{
					if (countPass==pass.length())
					{
						cout<<"----------------LOGGED IN!------------------------"<<endl;
						flage=true;
						break;
					}
					else if (countPass!=pass.length() && countName==name.length()) 
					{
						cout<<"Incorrect Password"<<endl;
						break;
					}
					
				}
				else if(countPass==pass.length() && countName!=name.length())
				{
					cout<<"Incorrect Username"<<endl;
				}
				else 
					continue;
			}
			infile.close();
			return flage;
		}
	
};
//Class to check students personal information
class studentPersonInfo: public checkLoginStudent
{
	public:
		studentPersonInfo(string name, string pass):checkLoginStudent(name, pass)
		{}
		
		bool check()
		{
			checkLoginStudent::check();
		}
		void printInfo()
		{
			string line;
			ifstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\"+convert(username)+"\\Personal_INFO.txt";
			file.open(fileName.c_str());
			while(!file.eof())
			{
				std::getline(file,line);
				cout<<line<<endl;
			}
		}
};

//Class to view students Study plan
class studyPlan:public checkLoginStudent
{
	public:
	studyPlan(string name, string pass):checkLoginStudent(name, pass)
	{}	
	
	void printStudyPlan()
	{
		string line;
		ifstream file("C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\Studyplan.txt");
		while(!file.eof())
		{
			std::getline(file, line);
			cout<<line<<endl;
		}
		file.close();
	}
};

//Teacher Class
class Teacher:public Person
{
	protected:
		string username;
		string password;
	public:
		Teacher(string name, string pass)
		{
			username=name;
			password=pass;	
		}
};
//Class to add/delete students attendence

//Class to check username and passwards of teachers
class checkLoginTeacher: public Teacher
{
	public:
		checkLoginTeacher(string name, string pass):Teacher(name, pass)
		{}
		
		bool check()
		{
			bool flag=false;
			string name;
			string pass;
			int countName=0;
			int countPass=0;
			ifstream infile;
			infile.open("C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\Teacherpasswords.txt");
			while(!infile.eof())
			{
				infile>>name;
				infile>>pass;
				countName=0;
				countPass=0;	
				for (int i=0; i<name.length(); i++)
				{
					if (username.at(i)==name.at(i))
					{
						countName++;
					}
					else 
						break;
				}
				for (int i=0; i<pass.length(); i++)
				{
					if (password.at(i)==pass.at(i))
					{
						countPass++;
					}
					else 
						break;
				}
				if (countName==name.length())
				{
					if (countPass==pass.length())
					{
					cout<<"----------------LOGGED IN!------------------------"<<endl;
						flag=true;
						break;
					}
					else if (countPass!=pass.length() && countName==name.length()) 
					{
						cout<<"Incorrect Password"<<endl;
						break;
					}
					
				}
				else if(countPass==pass.length() && countName!=name.length())
				{
					cout<<"Incorrect Username"<<endl;
				}
				else 
					continue;
			}
			infile.close();
			return flag;
			
		}
};

//Class to display attendence
class displayAttendence: public checkLoginTeacher
{
	public:
		displayAttendence(string name, string pass):checkLoginTeacher(name, pass)
		{}
		
		void display()
		{
			string line;
			ifstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\Attendence.txt";
			file.open(fileName.c_str());
			while(!file.eof())
			{
				std::getline(file,line);
				cout<<line<<endl;
			}
			file.close();	
		}
};

//Class to add/delete attendence
class teacherAttendence:public checkLoginTeacher
{
	protected:
		string **attend;
		int days;
		int numStudents;
		
	public:
		
		teacherAttendence(string name, string pass, int size):checkLoginTeacher(name, pass)
		{
			numStudents=10;
			numStudents+=1; //to store the word "NAME"
			days=size;
			attend= new string* [numStudents];
			for (int i=0; i<numStudents; i++)
			{
				attend[i]=new string [days];
			}
	 }
		void addNewAttendence()
		{
			string name[numStudents];
			string line;
			fstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\Attendence.txt";
			file.open(fileName.c_str(),ios::in);
			while(!file.eof())
			{
				for (int i=0; i<numStudents; i++)
				{
					file>>name[i];
					for (int j=0; j<days-1; j++)
					{
						file>>attend[i][j];
					}
				}
			}
			file.close();
			//adding new column
			for(int i=0;i<numStudents;i++)
			{
				if(i==0)
				{
					cout<<"DATE: ";
					cin>>attend[i][days-1];
				}
				else
				{
				cout<<name[i]<<" Attendence[P/A]: ";
				cin>>attend[i][days-1];
				}	
			}
			ofstream outfile(fileName.c_str());
			if(outfile.is_open())
			{
				for (int i=0; i<numStudents; i++)
				{
					outfile<<name[i]<<"\t";
					for (int j=0; j<days; j++)
					{
						if(i==0)
						outfile<<attend[i][j]<<"\t";
						else if(i!=0)
						outfile<<attend[i][j]<<"\t"<<"\t";
					}
					outfile<<endl;
				}
			}
			outfile.close();
		}
	void deleteAttendence()
		{
			string name[numStudents];
			string line;
			fstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\Attendence.txt";
			file.open(fileName.c_str(),ios::in);
			while(!file.eof())
			{
				for (int i=0; i<numStudents; i++)
				{
					file>>name[i];
					for (int j=0; j<days; j++)
					{
						file>>attend[i][j];
					}
				}
			}
			file.close();
			//searching for the date which will be removed
			string date;
			int index;
			cout<<"Enter the Date: ";
			cin>>date;
			for(int i=0; i<1; i++)
			{
				for (int j=0; j<days; j++)
				{
					if (date == attend[i][j])
					{
						index=j;
					}
				}
			}
			for (int i=0; i<numStudents; i++)
			{
				for (int j=0; j<=index; j++)
				{
					if (j==index)
					attend[i][j]="0";
				}
			}
			ofstream outfile(fileName.c_str());
			if(outfile.is_open())
			{
				for (int i=0; i<numStudents; i++)
				{
					outfile<<name[i]<<"\t";
					for (int j=0; j<days; j++)
					{
						if (attend[i][j] == "0") continue;
						else
						{
							if(i==0)
							outfile<<attend[i][j]<<"\t";
							else if(i!=0)
							outfile<<attend[i][j]<<"\t"<<"\t";
						}	
					}
					outfile<<endl;
				}
			}
			outfile.close();
		}
		
	//friend class studentAttendence;	

	~teacherAttendence()
{
	for (int i=0; i<numStudents; i++)
		delete [] attend[i];
	delete attend;	
}	
	friend void updateAttendence(teacherAttendence& obj);
};

//function to keep updating teachers and attendence send it to students folder
void updateAttendence(teacherAttendence &obj)
{
	int num;
	string *name;
	num=obj.numStudents-1;
	name=new string [num];
	ifstream file("C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\List_of_Students.txt");
	while(!file.eof())
	{
		for (int i=0; i<num; i++)
		{
			file>>name[i];
		}
	}
	file.close();

	string *date;
	date=new string [obj.days];
	for (int i=0; i<obj.days; i++)
	{
		date[i]=obj.attend[0][i];
//		cout<<date[i]<<"\t";
	}
	for (int i=0; i<obj.numStudents-1; i++)
	{
		for (int j=0; j<obj.days; j++)
		{
			obj.attend[i][j]=obj.attend[i+1][j];
		}
	}
	
//	for (int i=0; i<num; i++)
//	{
//		cout<<name[i]<<"\t";
//		for (int j=0; j<obj.days; j++)
//		{
//			cout<<obj.attend[i][j]<<"\t";
//		}
//		cout<<endl;
//	}
	string subject;
	string course;
		if (obj.username == "abbas")
			{
			course="============LINEAR ALGEBRA============";
			subject="Attendence_MT104.txt";
			}
		else if (obj.username == "ahmed")
			{
			course="============Computer Organization and Assembly Language============";
			subject="Attendence_EE213.txt";
			}
		else if (obj.username == "nouman")
			{
			subject="Attendence_CS211.txt";
			course="============Discrete Structure============";
			}	
		else if (obj.username == "safia")
			{
			subject="Attendence_CS217.txt";
			course="============Object Oreinted Programming============";
			}
		else if (obj.username == "shahzad")
			{
			subject="Attendence_CS217-LAB.txt";
			course="============Programming Fundamental============";
			}
	string fileName[10];
	for (int i=0; i<num; i++)
	{
		fileName[i]="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\"+convert(name[i])+"\\"+subject;
	//	cout<<fileName[i]<<endl;
	}
	for (int i=0; i<num; i++)
	{
		fstream outfile;
		outfile.open(fileName[i].c_str(),ios::out);
		if (outfile.is_open())
		{
			outfile<<course<<endl;
			for (int j=0; j<obj.days; j++)
			{
				if (date[j]=="0" && obj.attend[i][j]=="0")
				continue;
				else
				outfile<<date[j]<<"\t";
				outfile<<obj.attend[i][j]<<"\n";
			}
		}
		else
		{
			cout<<"File Doesnot Exist!"<<endl;
			break;
		}
		outfile.close();
	}
	
	delete [] name;
	delete [] date;
}

//Class to output the students addendence
class studentAttendence:public checkLoginStudent
{
	public:
		studentAttendence(string name, string pass): checkLoginStudent(name, pass)
		{}
		
		void displayAttendence()
		{
			string course[5];
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\"+convert(username)+"\\Courses.txt";
			ifstream infile(fileName.c_str());
			while(!infile.eof())
			{
				for (int i=0; i<5; i++)
				{
					infile>>course[i];
				}
			}
			infile.close();
			string line;
			for (int i=0; i<5; i++)
			{
				fileName= "C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\"+convert(username)+"\\Attendence_"+course[i]+".txt";
				ifstream file(fileName.c_str());
				while(!file.eof())
				{
					std::getline(file,line);
					cout<<line<<endl;
				}
				file.close();
			}
		}
};//Class to display marks
class displayMarks: public checkLoginTeacher
{
	public:
		displayMarks(string name, string pass):checkLoginTeacher(name, pass)
		{}
		
		void display()
		{
			string line;
			ifstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\Marks.txt";
			file.open(fileName.c_str());
			while(!file.eof())
			{
				std::getline(file,line);
				cout<<line<<endl;
			}
			file.close();	
		}
};
//Class to add/marks marks
class teacherMarks:public checkLoginTeacher
{
	protected:
		string **marks;
		int days;
		int numStudents;
	public:
		teacherMarks(string name, string pass, int size):checkLoginTeacher(name, pass)
		{
			numStudents=10;
			numStudents+=1; //to store the word "NAME"
			days=size;
			marks= new string* [numStudents];
			for (int i=0; i<numStudents; i++)
			{
				marks[i]=new string [days];
			}
	 }
		void addNewMarks()
		{
			string name[numStudents];
			string line;
			fstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\Marks.txt";
			file.open(fileName.c_str(),ios::in);
			while(!file.eof())
			{
				for (int i=0; i<numStudents; i++)
				{
					file>>name[i];
					for (int j=0; j<days-1; j++)
					{
						file>>marks[i][j];
					}
				}
			}
			file.close();
			//adding new column
			for(int i=0;i<numStudents;i++)
			{
				if(i==0)
				{
					cout<<"Next Lab/Assign/quiz etc... : ";
					cin>>marks[i][days-1];
				}
				else
				{
				cout<<name[i]<<" marks: ";
				cin>>marks[i][days-1];
				}	
			}
			ofstream outfile(fileName.c_str());
			if(outfile.is_open())
			{
				for (int i=0; i<numStudents; i++)
				{
					outfile<<name[i]<<"\t";
					for (int j=0; j<days; j++)
					{
						outfile<<marks[i][j]<<"\t"<<"\t";
					}
					outfile<<endl;
				}
			}
			outfile.close();
		}
	void deleteMarks()
		{
			string name[numStudents];
			string line;
			fstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\Marks.txt";
			file.open(fileName.c_str(),ios::in);
			while(!file.eof())
			{
				for (int i=0; i<numStudents; i++)
				{
					file>>name[i];
					for (int j=0; j<days; j++)
					{
						file>>marks[i][j];
					}
				}
			}
			file.close();
			//searching for the date which will be removed
			string lab;
			int index;
			cout<<"Enter the lab to be deleted: ";
			cin>>lab;
			for(int i=0; i<1; i++)
			{
				for (int j=0; j<days; j++)
				{
					if (lab == marks[i][j])
					{
						index=j;
					}
				}
			}
			for (int i=0; i<numStudents; i++)
			{
				for (int j=0; j<=index; j++)
				{
					if (j==index)
					marks[i][j]="0";
				}
			}
			ofstream outfile(fileName.c_str());
			if(outfile.is_open())
			{
				for (int i=0; i<numStudents; i++)
				{
					outfile<<name[i]<<"\t";
					for (int j=0; j<days; j++)
					{
						if (marks[i][j] == "0") continue;
						else
						{
							outfile<<marks[i][j]<<"\t"<<"\t";
						}	
					}
					outfile<<endl;
				}
			}
			outfile.close();
		}
		
~teacherMarks()
{
	for (int i=0; i<numStudents; i++)
		delete [] marks[i];
	delete marks;
}	
	friend void updateMarks(teacherMarks& obj);
};

void updateMarks(teacherMarks &obj)
{
	int num;
	string *name;
	num=obj.numStudents-1;
	name=new string [num];
	ifstream file("C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\List_of_Students.txt");
	while(!file.eof())
	{
		for (int i=0; i<num; i++)
		{
			file>>name[i];
		}
	}
	file.close();

	string *course;
	course=new string [obj.days];
	for (int i=0; i<obj.days; i++)
	{
		course[i]=obj.marks[0][i];
//		cout<<date[i]<<"\t";
	}
	for (int i=0; i<obj.numStudents-1; i++)
	{
		for (int j=0; j<obj.days; j++)
		{
			obj.marks[i][j]=obj.marks[i+1][j];
		}
	}

	string subject;
	string course_name;
		if (obj.username == "abbas")
			{
			course_name="============LINEAR ALGEBRA============";
			subject="Marks_MT104.txt";
			}
		else if (obj.username == "ahmed")
			{
			course_name="============Computer Organization and Assembly Language============";
			subject="Marks_EE213.txt";
			}
		else if (obj.username == "nouman")
			{
			subject="Marks_CS211.txt";
			course_name="============Discrete Structure============";
			}	
		else if (obj.username == "safia")
			{
			subject="Marks_CS217.txt";
			course_name="============Object Oreinted Programming============";
			}
		else if (obj.username == "shahzad")
			{
			subject="Marks_CS217-LAB.txt";
			course_name="============Programming Fundamental============";
			}

		//converting marks array from string to int
		
		int **M;//int marks dynamic array
		
		M= new int* [obj.numStudents];
			for (int i=0; i<obj.numStudents; i++)
			{
				M[i]=new int [obj.days];
			}
		
		for(int i=0;i<num;i++)
		{
			for(int j=0;j<obj.days;j++)
			{
				stringstream OBJ(obj.marks[i][j]); 
				OBJ>>M[i][j];
			}
		}
		//doing calculation through M array
		double sum=0;	//obtained total
		double G_total=0;	//Grand total
		double x=0; 	//percentage calculator
		string str="OBTAINED MARKS: ";
		string str1="GRAND TOTAL: ";
		string str2="PERCENTAGE: ";
		string str3="OBTAINED GRADE: ";
		char grade; 	//grade
		string fileName[10];
		//calculating total marks
	for (int i=0; i<num; i++)
	{
		fileName[i]="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\"+convert(name[i])+"\\"+subject;
		//cout<<fileName[i]<<endl;
	}
	for (int i=0; i<num; i++)
	{	
		//initializing variables to make it zero when new file opens
		sum=0;
		G_total=0;
		x=0;
		fstream outfile;
		outfile.open(fileName[i].c_str(),ios::out);
		if (outfile.is_open())
		{
			outfile<<course_name<<endl;
			
			for (int j=0; j<obj.days; j++)
			{
				sum+=M[i][j];
				G_total+=10.0;
				if (course[j]=="0" && obj.marks[i][j]=="0")
				{
				G_total-=10.0;
				continue;
				}
				else
				outfile<<course[j]<<"\t";
				outfile<<obj.marks[i][j]<<"\n";
			}
			outfile<<str<<sum<<endl;
			outfile<<str1<<G_total<<endl;
			x=(sum/G_total)*100.0;
			//calculating grade
			if(x>=90.0 && x<=100.0)
			grade='A';
			else if(x>=80.0 && x<=90.0)
			grade='B';
			else if(x>=70.0 && x<=80.0)
			grade='C';
			else if(x>=60.0 && x<=70.0)
			grade='D';
			else if(x>=50.0 && x<=60.0)
			grade='E';
			else if(x>=0 && x<=50.0)
			grade='F';			
			outfile<<str2<<x<<endl;
			outfile<<str3<<grade<<endl;
		}
		else
		{
			cout<<"File Doesnot Exist!"<<endl;
			break;
		}
		outfile.close();	
	}
		delete [] name;
		delete [] course;
	
}

class studentMarks: public checkLoginStudent
{
	private:
		string course[5];
	public:
		studentMarks(string name, string pass):checkLoginStudent(name,pass)
		{
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\"+convert(username)+"\\Courses.txt";
			ifstream infile(fileName.c_str());
			while(!infile.eof())
			{
				for (int i=0; i<5; i++)
				{
					infile>>course[i];
				}
			}
			infile.close();
		}
		
		void displayMarks()
		{
			string line;
			string fileName;
			for (int i=0; i<5; i++)
			{
				fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Student\\"+convert(username)+"\\Marks_"+course[i]+".txt";
				ifstream infile(fileName.c_str());
				while(!infile.eof())
				{
					std::getline(infile, line);
					cout<<line<<endl;
				}
				infile.close();
			}
		}	
};
//Class to check teachers personal information
class teacherPersonInfo: public checkLoginTeacher
{
	public:
		teacherPersonInfo(string name, string pass):checkLoginTeacher(name, pass)
		{}
		
		bool check()
		{
			checkLoginTeacher::check();
		}
		void printInfo()
		{
			string line;
			ifstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\Personal_INFO.txt";
			file.open(fileName.c_str());
			while(!file.eof())
			{
				std::getline(file,line);
				cout<<line<<endl;
			}
		}
};
//Class to display teaching plan
class teacherTeachingplan: public checkLoginTeacher
{
	public:
	teacherTeachingplan(string name, string pass):checkLoginTeacher(name, pass)
	{}	
	
	void printInfo()
		{
			string line;
			ifstream file;
			string fileName="C:\\Users\\Hp\\Downloads\\FAST\\3rd semester\\OOP\\MINIFLEX\\Teacher\\"+convert(username)+"\\TeachingPlan.txt";
			file.open(fileName.c_str());
			while(!file.eof())
			{
				std::getline(file,line);
				cout<<line<<endl;
			}
		}
};

int main()
{	
	Person *p;
	string name;
	string pass;
	int ask;
	char ch;
	cout<<"Are you a student or teacher? [Enter s/t]: ";
	cin>>ch;
	while(ch!='Q')
	{
		if(ch=='s'||ch=='S')
		{
			cout<<"----------------STUDENT FLEX----------------------"<<endl;
			cout<<"Enter Username [Your Last name]: ";
			cin>>name;
			cout<<"Enter Password: ";
			cin>>pass;
			checkLoginStudent S1(name, pass);
			p=&S1;
			bool result=p->check();
				if (result)
				{
					while(ch=='s')
					{
						cout<<"----------------WELCOME "<<name<<"---------------------"<<endl;
						cout<<"PRESS 1: to view your personal information"<<endl;
						cout<<"PRESS 2: to access your marks"<<endl;
						cout<<"PRESS 3: to check your attendence"<<endl;
						cout<<"PRESS 4: to view your study plan"<<endl;
						cout<<"PRESS 5: to quit"<<endl;
						
						cout<<"--------------------------------------------------"<<endl;
						cout<<"ENTER [1,2,3,4,5]: ";
						cin>>ask;
							if(ask==1)
							{
								cout<<endl;
								studentPersonInfo I1(name, pass);
								I1.printInfo();		
							}
							else if(ask==2)
							{
								cout<<endl;
								studentMarks SM1(name,pass);
								SM1.displayMarks();
							}
							else if(ask==3)
							{
								cout<<endl;
								
								studentAttendence SA1(name,pass);
								SA1.displayAttendence();
							}
							else if(ask==4)
							{
								cout<<endl;
								studyPlan A1(name,pass);
								A1.printStudyPlan();
							}
							else if(ask==5)
							{
								cout<<endl;
								cout<<"----------------SIGNED OUT FROM STUDENT FLEX------------------"<<endl;
								break;
							}
					}
				}
		}
	else if(ch=='t'||ch=='T')
	{
		cout<<"----------------TEACHER FLEX----------------------"<<endl;
		cout<<"Enter Username [Your Last name]: ";
		cin>>name;
		cout<<"Enter Password: ";
		cin>>pass;
		checkLoginTeacher T1(name, pass);
		p=&T1;
		bool result=p->check();
		if(result)
		{
			while(ch=='t')
			{
					cout<<"----------------WELCOME "<<name<<"-------------------"<<endl;
				cout<<"PRESS 1: to view your personal information"<<endl;
				cout<<"PRESS 2: to access students marks"<<endl;
				cout<<"PRESS 3: to check your students attendence"<<endl;
				cout<<"PRESS 4: to check your teaching plan"<<endl;
				cout<<"PRESS 5: to quit"<<endl;
				cout<<"--------------------------------------------------"<<endl;
				cout<<"ENTER [1,2,3,4,5]: ";
				cin>>ask;
					if(ask==1)
					{
						cout<<endl;
						teacherPersonInfo J1(name, pass);
						J1.printInfo();		
					}
					else if(ask==2)
					{
						cout<<endl;
						char ask1;
						cout<<"---------------------------------------------"<<endl;
						cout<<"PRESS A: to add New Marks"<<endl;
						cout<<"PRESS B: to Delete Marks"<<endl;
						cout<<"PRESS C: to view Marks"<<endl;
						cout<<"PRESS 0: to Exit"<<endl;
						cout<<"---------------------------------------------"<<endl;
						cout<<"ENTER [A, B, C, 0]: ";
						cin>>ask1;
						while(ask1!='0')
						{
							if (ask1=='A' || ask1=='a')
							{
								int lectureNum;
								cout<<"Enter the next LAB/CLASS NO: ";
								cin>>lectureNum;
								teacherMarks B1(name,pass,lectureNum);
								B1.addNewMarks();
								updateMarks(B1);
							}
							else if (ask1=='B' || ask1=='b')
							{
								int totalLec;
								cout<<"Enter the total number of lectures attended: ";
								cin>>totalLec;
								teacherMarks B2(name,pass,totalLec);
								B2.deleteMarks();
								updateMarks(B2);
							}
							else if (ask1=='C' || ask1=='c')
							{
								displayMarks B3(name, pass);
								B3.display();
							}
							cout<<endl;
							cout<<"---------------------------------------------"<<endl;
							cout<<"PRESS A: to Add New Marks"<<endl;
							cout<<"PRESS B: to Delete Marks"<<endl;
							cout<<"PRESS C: to View Marks"<<endl;
							cout<<"PRESS 0: to Exit"<<endl;
							cout<<"---------------------------------------------"<<endl;
							cout<<"ENTER [A, B, C, 0]: ";
							cin>>ask1;
						}
					}
					else if(ask==3)
					{
						//STUDENTS ATTENDENCE CLASS TO DO HERE 
						cout<<endl;
						char ask1;
						cout<<"---------------------------------------------"<<endl;
						cout<<"PRESS A: to add New Attendence"<<endl;
						cout<<"PRESS B: to Delete Attendence"<<endl;
						cout<<"PRESS C: to view Attendence"<<endl;
						cout<<"PRESS 0: to Exit"<<endl;
						cout<<"---------------------------------------------"<<endl;
						cout<<"ENTER [A, B, C, 0]: ";
						cin>>ask1;
						while(ask1!='0')
						{
							if (ask1=='A' || ask1=='a')
							{
								int lectureNum;
								cout<<"Enter the next Lecture number: ";
								cin>>lectureNum;
								teacherAttendence A1(name,pass,lectureNum);
								A1.addNewAttendence();
								updateAttendence(A1);
							}
							else if (ask1=='B' || ask1=='b')
							{
								int totalLec;
								cout<<"Enter total lectures attended: ";
								cin>>totalLec;
								teacherAttendence A2(name,pass,totalLec);	
								A2.deleteAttendence();
								updateAttendence(A2);
							}
							else if (ask1=='C' || ask1=='c')
							{
								displayAttendence D(name, pass);
								D.display();
							}
							cout<<endl;
							cout<<"---------------------------------------------"<<endl;
							cout<<"PRESS A: to Add New Attendence"<<endl;
							cout<<"PRESS B: to Delete Attendence"<<endl;
							cout<<"PRESS C: to View Attendence"<<endl;
							cout<<"PRESS 0: to Exit"<<endl;
							cout<<"---------------------------------------------"<<endl;
							cout<<"ENTER [A, B, C, 0]: ";
							cin>>ask1;
						}
					}
					else if(ask==4)
					{
						cout<<endl;
						teacherTeachingplan P1(name,pass);
						P1.printInfo();
					}
					else if(ask==5)
					{
						cout<<endl;
						cout<<"----------------SIGNED OUT FROM FLEX TEACHER-----------------"<<endl;
						break;
					}
			
			}
		}
		
	}
	else
	{
		cout<<"INVALID CHARACTER"<<endl;
	}
	cout<<"ENTER S/T again to continue else press 'Q' to quit"<<endl;
	cin>>ch;
	}
	cout<<"----------------------THE END----------------------"<<endl;
	return 0;
}