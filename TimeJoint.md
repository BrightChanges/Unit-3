# TimeJoint | Mini-IA
### Created by Kien Le Trung


## Criteria A: Planning
### Context of the product
My client is Owen, a student in my school. In conversation with Owen (see appendix A), he says that he has trouble with managing time for the huge amount of homeworks and tests he has at school. He says he would love me to create a software that could help him manage time better. 
Link to appendix A:...

#### Sketches of Ideas

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_0037.jpg)
Fig.1 Sketches of TimeJoint

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_0038.jpg)
Fig.2 Other sketches of TimeJoint

## Criteria B: Design
#### System Diagram

![](https://github.com/BrightChanges/Unit-3/blob/main/TimeJoint_systemdiagram%20(1).png)


#### Flow Diagram of the priority-algorithm (DO THIS DIGITALLY AFTER THE WHOLE PROGRAM WORKED)

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_0044.jpg)

#### ER Diagram (DO THIS DIGITALLY AFTER THE WHOLE PROGRAM WORKED)

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4833.jpg)

## Criteria C: Development
1st development story:


#### Codes (SO FAR <=REMOVE THIS ONCE COMPLETED):

-main.py:

```.py

##WORKING IN PROGRESS!!!
from kivy.lang import Builder
from sqlalchemy import Column, DateTime, String, Integer, ForeignKey
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship


from kivy.uix.behaviors import ButtonBehavior
from sqlalchemy.sql.functions import current_user

from kivymd.app import MDApp
from kivymd.uix.label import MDLabel
from kivymd.uix.picker import MDDatePicker
from kivymd.uix.screen import MDScreen
from datetime import date
from kivymd.uix.list import OneLineListItem


Base = declarative_base()




class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, unique=True)
    email = Column(String(256),  unique=True)
    username = Column(String(256))
    password = Column(String)

    #relationship "has in the ER diagram" one-to-many
    tasks = relationship("Tasks", backref="creator")

class Tasks(Base):
    __tablename__ = "tasks"
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String)
    deadline = Column(String)
    type = Column(String)
    user_id = Column(Integer, ForeignKey('user.id'))


#create the database and the connection
from sqlalchemy import create_engine
engine = create_engine('sqlite:///maintimejoint.db')

from sqlalchemy.orm import sessionmaker
session = sessionmaker()
session.configure(bind=engine)
Base.metadata.create_all(engine)


# class MyTaskScreen(MDScreen):


class TaskScreen(MDScreen):
    def __init__(self, **kw):
        super().__init__(**kw)
        self.date = ""
        self.id = LoginScreen.current_user

    value_hold=0

    def show_task(self):
        #LoginScreen.current_user is the id of the current user.
        post_list = ""
        s = session()
        post_check = s.query(Tasks).filter_by(user_id=LoginScreen.current_user).all()

        for i in range(len(post_check)):
            # print(post_check[i].name, "|" ,post_check[i].deadline,  "|" , post_check[i].type)
            post_list +=  ","+ str(post_check[i].name) + "|" +str(post_check[i].deadline)+ "|"+str(post_check[i].type)
        print(post_list)



    def add_task(self):
        global today

        user_id = LoginScreen.current_user
        value_hold = self.date
        task_name = self.ids.task_name.text
        type = self.ids.type.text
        deadline = value_hold
        print(task_name)
        print(type)
        print(deadline)

#validate if the input is valid, preventing error
        if type != "Test" and type != "test" and type!= "Homework" and type != "homework":
            print("Invalid type for task!")
        elif len(task_name)>256:
            print("Task name is too long!")
        elif str(deadline) < str(date.today()):
            print("Task's deadline is invalid!")
        else:
            s = session()
            task = Tasks(name=task_name, deadline=deadline,type=type, user_id=user_id)
            s.add(task)
            print("task with name {} , deadline {}, type {} was added to database".format(task_name,deadline,type))
            s.commit()
            s.close()







    def on_save(self, instance, value, date_range):
        self.date = value


    def on_cancel(self, instance, value):
        """Events called when the "CANCEL" dialog box button is clicked."""

    def show_date_picker(self):

        date_dialog = MDDatePicker()
        date_dialog.bind(on_save=self.on_save, on_cancel=self.on_cancel)
        date_dialog.open()
        return self.on_save





class HomeScreen(MDScreen):

    def try_prioritize(self):
        print("Prioritize button clicked")

    def proceed_to_add_task(self):
        print("Add task button (non-active one) clicked")
        self.parent.current = "TaskScreen"



class RegisterScreen(MDScreen):


    def try_register(self):

        print("Register was attempted")
        username = self.ids.username_input.text
        email = self.ids.email_input.text
        password_add = self.ids.password_input.text
        password_repeat = self.ids.password_check.text
        print(username, email, password_add, password_repeat)

        if password_add == password_repeat:
            s = session()
            username = self.ids.username_input.text
            email = self.ids.email_input.text
            password = self.ids.password_input.text
            print(username,email, password)

            email_check = s.query(User).filter_by(email=email).first() ##similar to fetchone()

            if email_check:
                print("User already exists")
                s.close()

            else:
                user = User(username=username, email=email, password=password)
                s.add(user)

                s.commit()
                s.close()




                self.parent.current = "LoginScreen"

        else:
            print("The passwords do not match")


class ButtonLabel(ButtonBehavior, MDLabel):
    pass

class LoginScreen(MDScreen):
    current_user = None

    def try_login(self):
        email = self.ids.email_input.text
        password = self.ids.password_input.text

        s = session()
        #THIS QUERY CODE BELOW only work if I add .first()=>ask Dr Ruben why so?
        user_check = s.query(User).filter_by(email=email,password=password).first()
        print(user_check)

        if user_check:
            s.close()
            print(f"login successful for user with email '{email}")
            LoginScreen.current_user = user_check.id #getting the ide of the current user
            self.parent.current = "HomeScreen"

        else:
            print("User does not exist")

class MainApp(MDApp):
    def build(self):
        self.theme_cls.primary_palette = "Orange"


        return


MainApp().run()
```

-main.kv:

```.py
ScreenManager:
    id: scr_manager

    LoginScreen:
        name: "LoginScreen"

    RegisterScreen:
        name: "RegisterScreen"

    HomeScreen:
        name: "HomeScreen"







<TaskScreen>:
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "lake.jpg"



    MDCard:
        size_hint: 0.5, 0.75
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"

#How to change the c0lor of background of a MDCard

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)



            MDLabel:
                text: "TimeJoint"
                font_style: "H4"
                halign: "center"

            MDRaisedButton:
                text: "Check my task"
                pos_hint: {'center_x': .5, 'center_y': .5}
                on_release:
                    root.show_task()


            ScrollView:

                MDList:
                    id: task_list

            MDLabel:
                text: "Add task"
                font_style: "H6"
                halign: "center"

            MDTextField:
                id: task_name
                hint_text: "Task/Homework/Test Name"

                required: True



            MDRaisedButton:
                text: "Pick Deadline"
                pos_hint: {'center_x': .5, 'center_y': .5}
                on_release:
                    root.show_date_picker()



            MDTextField:
                id: type
                hint_text: "Type (Test or Homework?)"


                required: True

            MDRaisedButton:
                text: "+ Add task"
                on_release:
                    root.add_task()




<HomeScreen>:
#this is for the visual
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "lake.jpg"

    MDCard:
        size_hint: 0.5, 0.75
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"

#How to change the c0lor of background of a MDCard

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)

            MDLabel:
                text: "TimeJoint"
                font_style: "H4"
                halign: "center"

            MDLabel:

            MDRaisedButton:
                text: "View your tasks in prioritize order"
                on_release:
                    root.try_prioritize()

            MDRaisedButton:
                text: "Add task"
                on_release:
                    root.proceed_to_add_task()

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:








#the RegisterScreen
<RegisterScreen>:
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "lake.jpg"

    MDCard:
        size_hint: 0.5, 0.75
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"

#How to change the c0lor of background of a MDCard

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)

            MDLabel:
                text: "REGISTER"
                font_style: "H3"
                halign: "center"


            MDTextField:
                id: username_input
                hint_text: "Username"

                required: True

            MDTextField:
                id: email_input
                hint_text: "Email"
                icon_left: "email"
                helper_text: "Invalid email"
                helper_text_mode: "on_error"
                required: True

            MDTextField:
                id:password_input
                hint_text: "Password"
                icon_right: "eye-off"
                helper_text: "Invalid password"
                helper_text_mode: "on_error"
                required: True
                password: True

            MDTextField:
                id:password_check
                hint_text: "Password Check"
                icon_right: "eye-off"
                helper_text: "Invalid password"
                helper_text_mode: "on_error"
                required: True
                password: True

            MDRaisedButton:
                text: "Register"
                on_release:
                    root.try_register()






#The LoginScreen
<LoginScreen>:
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "lake.jpg"

    MDCard:
        size_hint: 0.5, 0.75
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"

        #Image:
            #source: "timejointicon.jpg"
            #size: self.width + 20, self.height + 20

#How to change the c0lor of background of a MDCard

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)

            MDLabel:
                text: "TimeJoint"
                font_style: "H3"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:
                text: "LOGIN"
                font_style: "H4"
                halign: "center"

            MDTextField:
                id: email_input
                hint_text: "Email"
                icon_left: "email"
                helper_text: "Invalid email"
                helper_text_mode: "on_error"
                required: True

            MDTextField:
                id:password_input
                hint_text: "Password"
                icon_right: "eye-off"
                helper_text: "Invalid password"
                helper_text_mode: "on_error"
                required: True
                password: True

            MDRaisedButton:
                text: "Log in"
                on_release:
                    root.try_login()

            MDBoxLayout:
                adaptive_height: True
                halign: "right"

                ButtonLabel:
                    text: "REGISTER"
                    on_press:
                        print("here")
                        root.parent.current = "RegisterScreen"



```

2st development story:


## Criteria D: Functionality
#### Alpha Testing:
Link to the desmontration video:...

#### Beta Testing with colleagues:
Link to the desmontration video:...

#### Acceptance Testing with client:
Link to the desmontration video:...

...
Fig. Picture 1 of TimeJoint 

...
Fig. Picture 2 of TimeJoint 

...
Fig. Picture 3 of TimeJoint 



## Criteria E: Evaluation
##### Success Criteria
...

