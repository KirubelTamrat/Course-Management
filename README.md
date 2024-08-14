import random

class Course:
    '''
        >>> c1 = Course('CMPSC132', 'Programming in Python II', 3)
        >>> c2 = Course('CMPSC360', 'Discrete Mathematics', 3)
        >>> c1 == c2
        False
        >>> c3 = Course('CMPSC132', 'Programming in Python II', 3)
        >>> c1 == c3
        True
        >>> c1
        CMPSC132(3): Programming in Python II
        >>> c2
        CMPSC360(3): Discrete Mathematics
        >>> c3
        CMPSC132(3): Programming in Python II
        >>> c1 == None
        False
        >>> print(c1)
        CMPSC132(3): Programming in Python II
    '''

    def __init__(self, cid, cname, credits):
        #initializes the course name 
        self.cname = cname
        #initialzes the course id 
        self.cid = cid 
        #initializes the number of credits
        self.credits = credits

    def __str__(self):
        #returns a formatted summary of the course as a string
        return f"{self.cid}({self.credits}): {self.cname}"

    __repr__ = __str__

    def __eq__(self, other):
        #checks if the other object is an instance of the course class
        if isinstance(other,Course):
            #compate course ids to return True
            return self.cid == other.cid
        else:
            #return false if other object is not a course object
            return False


class Catalog:
    ''' 
        >>> C = Catalog()
        >>> C.courseOfferings
        {}
        >>> C._loadCatalog("cmpsc_catalog_small.csv")
        >>> C.courseOfferings
        {'CMPSC 132': CMPSC 132(3): Programming and Computation II, 'MATH 230': MATH 230(4): Calculus and Vector Analysis, 'PHYS 213': PHYS 213(2): General Physics, 'CMPEN 270': CMPEN 270(4): Digital Design, 'CMPSC 311': CMPSC 311(3): Introduction to Systems Programming, 'CMPSC 360': CMPSC 360(3): Discrete Mathematics for Computer Science}
        >>> C.removeCourse('CMPSC 360')
        'Course removed successfully'
        >>> C.courseOfferings
        {'CMPSC 132': CMPSC 132(3): Programming and Computation II, 'MATH 230': MATH 230(4): Calculus and Vector Analysis, 'PHYS 213': PHYS 213(2): General Physics, 'CMPEN 270': CMPEN 270(4): Digital Design, 'CMPSC 311': CMPSC 311(3): Introduction to Systems Programming}
        >>> isinstance(C.courseOfferings['CMPSC 132'], Course)
        True
    '''

    def __init__(self):
        #creates empty dictionary for the course offerings
        self.courseOfferings = {}

    def addCourse(self, cid, cname, credits):
        self.cid = cid 
        self.cname = cname
        self.credits = credits
        #checks if the course id is already in the dictionary of course offerings
        if (cid in self.courseOfferings):
            return "Course already added"
        else:
            #creates a new course object
            course = Course(cid, cname, credits)
            #adds the course to the course offerings
            self.courseOfferings[cid] = course
            return "Course added successfully"

    def removeCourse(self, cid):
        #checks if the course id is already in the dictionary of course offerings
        if (cid in self.courseOfferings):
            #deletes the course 
            del self.courseOfferings[cid]
            return "Course removed successfully"
        else:
            #returns if course id isnt found in course offerings
            return "Course not found"
            

    def _loadCatalog(self, file):
        with open(file, 'r') as f: 
            course_info = f.readlines()  
        courses = [course.strip().split(',') for course in course_info]
        for course in courses:
            cid, cname, credits = course
            #converts credits to an int
            credits = int(credits)
            #adds the course to the catalog
            self.addCourse(cid, cname, credits)

                
 
class Semester:
    '''
        >>> cmpsc131 = Course('CMPSC 131', 'Programming in Python I', 3)
        >>> cmpsc132 = Course('CMPSC 132', 'Programming in Python II', 3)
        >>> math230 = Course("MATH 230", 'Calculus', 4)
        >>> phys213 = Course("PHYS 213", 'General Physics', 2)
        >>> econ102 = Course("ECON 102", 'Intro to Economics', 3)
        >>> phil119 = Course("PHIL 119", 'Ethical Leadership', 3)
        >>> spr22 = Semester()
        >>> spr22
        No courses
        >>> spr22.addCourse(cmpsc132)
        >>> isinstance(spr22.courses['CMPSC 132'], Course)
        True
        >>> spr22.addCourse(math230)
        >>> spr22
        CMPSC 132; MATH 230
        >>> spr22.isFullTime
        False
        >>> spr22.totalCredits
        7
        >>> spr22.addCourse(phys213)
        >>> spr22.addCourse(econ102)
        >>> spr22.addCourse(econ102)
        'Course already added'
        >>> spr22.addCourse(phil119)
        >>> spr22.isFullTime
        True
        >>> spr22.dropCourse(phil119)
        >>> spr22.addCourse(Course("JAPNS 001", 'Japanese I', 4))
        >>> spr22.totalCredits
        16
        >>> spr22.dropCourse(cmpsc131)
        'No such course'
        >>> spr22.courses
        {'CMPSC 132': CMPSC 132(3): Programming in Python II, 'MATH 230': MATH 230(4): Calculus, 'PHYS 213': PHYS 213(2): General Physics, 'ECON 102': ECON 102(3): Intro to Economics, 'JAPNS 001': JAPNS 001(4): Japanese I}
    '''


    def __init__(self):
        #dictionary to store courses for the semester
        self.courses = {}


    def __str__(self):
        #checks if there are no courses in self.courses
        if (len(self.courses) == 0):
            return "No courses"
        else:
            #join course ids to make a formatted summary using semicolons
            return "; ".join(self.courses.keys())

    __repr__ = __str__

    def addCourse(self, course):
        #checks if course id is in the self.courses dictionary
        if(course.cid in self.courses):
            return "Course already added"
        
        #adds the course to the self.courses dictionary
        self.courses[course.cid] = course

    def dropCourse(self, course):
        #checks if the course id is in the course dictiionary
        if (course.cid in self.courses):
            #deletes the course from self.courses
            del self.courses[course.cid]
        else:
            return "No such course"

    @property
    def totalCredits(self):
        #sums the credits of the courses in self.courses
        return sum(course.credits for course in self.courses.values())

    @property
    def isFullTime(self):
        #checks if total credits are greater than or equal to 12
        return self.totalCredits >= 12

    
class Loan:
    '''
        >>> import random
        >>> random.seed(2)  # Setting seed to a fixed value, so you can predict what numbers the random module will generate
        >>> first_loan = Loan(4000)
        >>> first_loan
        Balance: $4000
        >>> first_loan.loan_id
        17412
        >>> second_loan = Loan(6000)
        >>> second_loan.amount
        6000
        >>> second_loan.loan_id
        22004
        >>> third_loan = Loan(1000)
        >>> third_loan.loan_id
        21124
    '''
    

    def __init__(self, amount):
        self.amount = amount
        self.loan_id = self.__getloanID


    def __str__(self):
        #returns a formatted summary of the loan as a string
        return f"Balance: ${self.amount}"

    __repr__ = __str__


    @property
    def __getloanID(self):
        #creates a random loan id between 10,000 and 99,999
        return random.randint(10000,99999)


class Person:
    '''
        >>> p1 = Person('Jason Lee', '204-99-2890')
        >>> p2 = Person('Karen Lee', '247-01-2670')
        >>> p1
        Person(Jason Lee, ***-**-2890)
        >>> p2
        Person(Karen Lee, ***-**-2670)
        >>> p3 = Person('Karen Smith', '247-01-2670')
        >>> p3
        Person(Karen Smith, ***-**-2670)
        >>> p2 == p3
        True
        >>> p1 == p2
        False
    '''

    def __init__(self, name, ssn):
        #initializes person object with name 
        self.name = name 
        #initializes person object with ssn
        self.__ssn = ssn

    def __str__(self):
        #returns a string representaition of person object 
        return f"Person({self.name}, ***-**-{self.__ssn[-4:]})"

    __repr__ = __str__

    def get_ssn(self):
        #returns ssn 
        return self.__ssn

    def __eq__(self, other):
        #checks if two person objects are equal based of ssn 
        if isinstance(other,Person):
            return self.__ssn == other.__ssn
        #checks if other object is not a person 
        else:
            return False


class Staff(Person):
    '''
        >>> C = Catalog()
        >>> C._loadCatalog("cmpsc_catalog_small.csv")
        >>> s1 = Staff('Jane Doe', '214-49-2890')
        >>> s1.getSupervisor
        >>> s2 = Staff('John Doe', '614-49-6590', s1)
        >>> s2.getSupervisor
        Staff(Jane Doe, 905jd2890)
        >>> s1 == s2
        False
        >>> s2.id
        '905jd6590'
        >>> p = Person('Jason Smith', '221-11-2629')
        >>> st1 = s1.createStudent(p)
        >>> isinstance(st1, Student)
        True
        >>> s2.applyHold(st1)
        'Completed!'
        >>> st1.registerSemester()
        'Unsuccessful operation'
        >>> s2.removeHold(st1)
        'Completed!'
        >>> st1.registerSemester()
        >>> st1.enrollCourse('CMPSC 132', C)
        'Course added successfully'
        >>> st1.semesters
        {1: CMPSC 132}
        >>> s1.applyHold(st1)
        'Completed!'
        >>> st1.enrollCourse('CMPSC 360', C)
        'Unsuccessful operation'
        >>> st1.semesters
        {1: CMPSC 132}
    '''
    
    
    def __init__(self, name, ssn, supervisor=None):
        #calls constructer of parent class to set name and ssn
        super().__init__(name,ssn)
        #sets supervisor attribute 
        self.__supervisor = supervisor


    def __str__(self):
        #returns a string of staff object including name and id
        return f"Staff({self.name}, {self.id})"

    __repr__ = __str__


    @property
    def id(self):
        #creates an identifier for staff object based on last 4 digits of ssn and initials 
        initials = ''.join(name_part[0].lower() for name_part in self.name.split())
        return f"905{initials}{self.__ssn[-4:]}"

    @property   
    def getSupervisor(self):
        #returns supervisor 
        return self.__supervisor

    def setSupervisor(self, new_supervisor):
        #chekcs if supervisor is a staff object
        if isinstance(new_supervisor,Staff):
            #sets a new supervisor for the staff object 
            self.__supervisor = new_supervisor
            return "Completed!"


    def applyHold(self, student):
        #checks if the input is a student object
        if isinstance(student,Student):
            #applys the hold 
            student.hold = True
            return "Completed!"

    def removeHold(self, student):
        #chekcs if the input is a student object 
        if isinstance(student,Student):
            #removes hold 
            student.hold = False
            return "Completed!"

    def unenrollStudent(self, student):
         #checks if the input is a student object 
        if isinstance(student,Student):
            #unenroll student
            student.active = False
            return "Completed!"

    def createStudent(self, person):
        #creates a new student object based on person object 
        if isinstance(person, Person):
            return Student(person.name, person.__ssn, 'Freshman')
    


class Student(Person):
    '''
        >>> C = Catalog()
        >>> C._loadCatalog("cmpsc_catalog_small.csv")
        >>> s1 = Student('Jason Lee', '204-99-2890', 'Freshman')
        >>> s1
        Student(Jason Lee, jl2890, Freshman)
        >>> s2 = Student('Karen Lee', '247-01-2670', 'Freshman')
        >>> s2
        Student(Karen Lee, kl2670, Freshman)
        >>> s1 == s2
        False
        >>> s1.id
        'jl2890'
        >>> s2.id
        'kl2670'
        >>> s1.registerSemester()
        >>> s1.enrollCourse('CMPSC 132', C)
        'Course added successfully'
        >>> s1.semesters
        {1: CMPSC 132}
        >>> s1.enrollCourse('CMPSC 360', C)
        'Course added successfully'
        >>> s1.enrollCourse('CMPSC 465', C)
        'Course not found'
        >>> s1.semesters
        {1: CMPSC 132; CMPSC 360}
        >>> s2.semesters
        {}
        >>> s1.enrollCourse('CMPSC 132', C)
        'Course already enrolled'
        >>> s1.dropCourse('CMPSC 360')
        'Course dropped successfully'
        >>> s1.dropCourse('CMPSC 360')
        'Course not found'
        >>> s1.semesters
        {1: CMPSC 132}
        >>> s1.registerSemester()
        >>> s1.semesters
        {1: CMPSC 132, 2: No courses}
        >>> s1.enrollCourse('CMPSC 360', C)
        'Course added successfully'
        >>> s1.semesters
        {1: CMPSC 132, 2: CMPSC 360}
        >>> s1.registerSemester()
        >>> s1.semesters
        {1: CMPSC 132, 2: CMPSC 360, 3: No courses}
        >>> s1
        Student(Jason Lee, jl2890, Sophomore)
        >>> s1.classCode
        'Sophomore'
    '''
        
    
    def __init__(self, name, ssn, year):
        random.seed(1)
        #calls parent class to set name and ssn
        super().__init__(name, ssn)
        #sets academic year 
        self.classCode = year
        #initializes empty dictionary for semesters
        self.semesters = {}
        #intializes hold value to false
        self.hold = False
        #initiallizes active status to true 
        self.active = True
        #creates student account
        self.account = self.__createStudentAccount()

    def __str__(self):
        #returns string of student object 
        return f"Student({self.name}, {self.id}, {self.classCode})"

    __repr__ = __str__

    def __createStudentAccount(self):
        #returns the created student account 
        return StudentAccount(self)


    @property
    def id(self):
        #creates indentifier for the student object
        initials = ''.join(name_part[0].lower() for name_part in self.name.split())
        return f"{initials}{self.__ssn[-4:]}"

    def registerSemester(self):
        #checks if student is not active or if their account is on hold
        if not self.active or self.hold:
            return "Unsuccessful operation"
        
        #determines key for next semester
        next_semester_key = max(self.semesters.keys(), default = 0) + 1
        #creates a new semester and adds it to the dictionary 
        self.semesters[next_semester_key] = Semester()
        #updates academic year 
        self.updateClassCode()

    def updateClassCode(self):
        #updates academic year based on how many semesters completed
        num_of_semesters = len(self.semesters)
        if num_of_semesters <= 2:
            self.classCode = 'Freshman'
        elif num_of_semesters <= 4:
            self.classCode = 'Sophomore'
        elif num_of_semesters <= 6:
            self.classCode = 'Junior'
        else:
            self.classCode = 'Senior'



    def enrollCourse(self, cid, catalog):
        #checks if student is not active or if their account is on hold
        if not self.active or self.hold:
            return "Unsuccesful operation"
        
        #checks if the course exists in the course offerings
        if cid in catalog.courseOfferings:
            course = catalog.courseOfferings[cid]
        
        #if it doesnt exist there is no course found 
        else:
            return "Course not found"
        
        #gets the current semester 
        current_semester = self.semesters.get(max(self.semesters.keys(), default = 0))

        #checks if course is already enrolled 
        if course in list(current_semester.courses.values()):
            return "Course already enrolled"
        
        #adds the course to current semester 
        current_semester.addCourse(course)
        #changes the student's account 
        self.account.chargeAccount(self.account.CREDIT_PRICE * course.credits)

        return "Course added successfully"
    
    def dropCourse(self, cid):
        #checks if student is not active or if their account is on hold
        if not self.active or self.hold:
            return "Unsuccesful operation"
        
        #gets the current semester
        current_semester = self.semesters.get(max(self.semesters.keys(), default = 0))

        #checks if the course exists in the course offerings
        if cid not in current_semester.courses:
            return "Course not found"
        
        #calculates the refund amount and updates the student's account
        self.account.chargeAccount((current_semester.courses[cid].credits * self.account.CREDIT_PRICE / 2) * -1)
        
        #deletes course from the current semester 
        del current_semester.courses[cid]  
        
        return "Course dropped successfully"

    def getLoan(self, amount):
        #checks if the course exists in the course offerings
        if not self.active or self.hold:
            return "Unsuccessful operation"
        
        #gets current semester 
        current_semester = self.semesters.get(max(self.semesters.keys(), default = 0))

        #checks if the student is full time in the current semester
        if not current_semester.isFullTime:  
            return "Not full-time"
        
        #creates a new loan
        loan = Loan(amount)  
        
        #adds it to the student's account 
        self.account.loans[loan.loan_id] = loan  
        self.account.makePayment(amount)  



class StudentAccount:
    '''
        >>> C = Catalog()
        >>> C._loadCatalog("cmpsc_catalog_small.csv")
        >>> s1 = Student('Jason Lee', '204-99-2890', 'Freshman')
        >>> s1.registerSemester()
        >>> s1.enrollCourse('CMPSC 132', C)
        'Course added successfully'
        >>> s1.account.balance
        3000
        >>> s1.enrollCourse('CMPSC 360', C)
        'Course added successfully'
        >>> s1.account.balance
        6000
        >>> s1.enrollCourse('MATH 230', C)
        'Course added successfully'
        >>> s1.enrollCourse('PHYS 213', C)
        'Course added successfully'
        >>> print(s1.account)
        Name: Jason Lee
        ID: jl2890
        Balance: $12000
        >>> s1.account.chargeAccount(100)
        12100
        >>> s1.account.balance
        12100
        >>> s1.account.makePayment(200)
        11900
        >>> s1.getLoan(4000)
        >>> s1.account.balance
        7900
        >>> s1.getLoan(8000)
        >>> s1.account.balance
        -100
        >>> s1.enrollCourse('CMPEN 270', C)
        'Course added successfully'
        >>> s1.account.balance
        3900
        >>> s1.dropCourse('CMPEN 270')
        'Course dropped successfully'
        >>> s1.account.balance
        1900.0
        >>> s1.account.loans
        {27611: Balance: $4000, 84606: Balance: $8000}
        >>> StudentAccount.CREDIT_PRICE = 1500
        >>> s2 = Student('Thomas Wang', '123-45-6789', 'Freshman')
        >>> s2.registerSemester()
        >>> s2.enrollCourse('CMPSC 132', C)
        'Course added successfully'
        >>> s2.account.balance
        4500
        >>> s1.enrollCourse('CMPEN 270', C)
        'Course added successfully'
        >>> s1.account.balance
        7900.0
    '''
    
    
    CREDIT_PRICE = 1000
    def __init__(self, student):
        #intializes student attribute
        self.student = student
        #initializing balance to 0 
        self.balance = 0
        #initializes to store loans to empty dictionary
        self.loans = {}

    def __str__(self):
        #returns string containing student name, id, and balance
        return f"Name: {self.student.name}\nID: {self.student.id}\nBalance: ${self.balance}"

    __repr__ = __str__


    def makePayment(self, amount):
        # YOUR CODE STARTS HERE
        #subtracts balancce by the amount 
        self.balance -= amount
        #returns updated balance 
        return self.balance


    def chargeAccount(self, amount):
        #adds balance by the amount 
        self.balance += amount 
        #returns updated value
        return self.balance

    def addLoan(self,loan_id,amount):
        #adds loan to the loans dictionary with the loan id as the key
        self.loans[loan_id] = f"Balance: ${amount}"
        #subracts the loan amount from the balance
        self.balance -= amount

    def removeLoan(self,loan_id):
        #chekcs if the loan id exists in the loans dictionary
        if loan_id in self.loans:
           #gets the loan amount from loan entry
           amount = int(self.loans[loan_id].split('$')[1])
           #adds the loan amount to the balance
           self.balance += amount
           #deletes the loan entry from the loans dictionary
           del self.loans[loan_id]


def run_tests():
    import doctest

    # Run tests in all docstrings
    doctest.testmod(verbose=True)
    
    # Run tests per function - Uncomment the next line to run doctest by function. Replace Course with the name of the function you want to test
    doctest.run_docstring_examples(Course, globals(), name='HW2',verbose=True)   

if __name__ == "__main__":
    run_tests()
