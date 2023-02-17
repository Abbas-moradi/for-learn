import shortuuid


class User:
    def __init__(self, name, phone, password, email, location=None):
        self.name = name
        self.phone = phone
        # self.email = self.checker(email)
        self._email = email
        self._password = password
        self.location = location

    @property
    def password(self):
        return self._password

    @password.setter
    def password(self, password):
        if len(password) > 8:
            flag_digit = False
            flag_alpha = False
            for item in password:
                if item.isdigit():
                    flag_digit = True
                if item.isalpha():
                    flag_alpha = True
            if flag_alpha is False or flag_digit is False:
                raise ValueError("Sorry your password has wrong characters")
            else:
                self._password = password
        else:
            raise ValueError("Sorry your password length is low")
    #
    # def checker(self, email):
    #     if '@gmail.com' in email:
    #         return email
    #     raise ValueError('')

    @property
    def email(self):
        return self._email

    @email.setter
    def email(self, email1):
        if '@gmail.com' not in email1:
            raise ValueError('wrong email format')
        else:
            self._email = email1

class Staff(User):
    def __init__(self, name, phone, email, password, location_name, location_info, salary):
        super().__init__(self, name, phone, email, password)
        self.location_name = location_name
        self.location_info = location_info
        self.salary = salary

    def final_salary(self):
        result = 0.93 * self.salary
        if self.salary > 5000000:
            result = result - ((self.salary - 5000000) * 0.09)
        self.salary = result


class Course:
    def __init__(self, name, teacher_list, teacher):
        self.name = name
        self.teacher_list = teacher_list
        self.teacher = teacher
        self.id = shortuuid.uuid()

class Teacher(Staff):
    def __init__(self, facility, course_list, grade, course_term, salary, name, phone, email,
        password, location_name, location_info):
        super().__init__(name, phone, email, password, location_name, location_info, salary)
        self.facility = facility
        self.course_list = course_list
        self.grade = grade
        self.course_term = course_term
        self.salary_grade = self.wage()

    def add_course(self, course_name):
        if type(course_name) == list:
            self.course_list = self.course_list.extend(course_name)
        else:
            self.course_list.append(course_name)

    def wage(self):
        super().final_salary()
        if self.grade == 'TA':
            self.salary = self.salary * 1.15
        elif self.grade == 'PF':
            self.salary = self.salary * 1.3
        elif self.grade == 'TR':
            self.salary = self.salary * 1.2


class Student(User):
    def __init__(self, name, phone, email, password, course_list, course_score_list):
        super().__init__(self, name, phone, email, password)
        self.id = shortuuid.uuid()
        self.course_list = course_list
        self.course_score_list = course_score_list
        self.average = self.average_s()

    def get_score(self, course, increase):
       self.course_score_list[course] = increase

    def show_score(self, course):
        if course in self.course_list:
            return self.course_score_list.get(course)
        else:
            raise ValueError('the course does not exist')
    def average_s(self):
        sum = 0
        for j in self.course_score_list.values():
            sum += j
            print(j)
        average = sum / len(self.course_score_list)
        return average

U1 = User('hesam', 2365489, 'neyshaboor', 'hesam1234', 'a@gmail.com')
print(U1.password)
U1.password = 'javad9876'
print(U1.password)
U1.email = 'qw@gmail.com'
print(U1.email)

C1 = Course('physics', ['javad', 'hesam'], 'javad')
C2 = Course('math', ['javad', 'ali'], 'ali')

U2 = Teacher('loptop', ['physics', 'math'], 'TR', C1, 12000000, 'javad', 3654987, 'g@gmail.com',
        'password2', 'tehran', 'london')

st = Student('hesam', 5654896, 'h@gmail.com', 'password65', [C1, C2], {'physics': 14, 'math': 15.5})

# sf = Staff('alireza', 52365412, 'as@gmail.com', 'password5698', 'alborz', 'tehran', 9500000)
sf = Staff('ali', 365421, 'aw@gmail.com', 'pass56548', 'alborz', 'tehran', 9500000)
print(sf.final_salary())
print(U2.final_salary())
print(st.get_score('physics', 16))
print(st.average_s())
