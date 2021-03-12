# CoinSnack | Mini-IA
### Created by Kien Le Trung


## Criteria A: Planning
### Context of the product
My client is Owen, a student in my school. In conversation with Owen (see appendix A), Owen says that he can buy snack from the cafeteria, but sometimes, he don’t want to go to the cafeteria to buy them and just want to stay indoor. So, he would want me to create a communication system that takes his order online and send that order to the cafeteria, where its staff could do further procedure.

Link to appendix A:  https://github.com/BrightChanges/Unit-3/blob/main/CS%20Mini%20IA%20interview.m4a

Listening to this interview, I identified the problem that Owen was having: There is no online communication system between him and the cafeteria. So, I decided to come up with a solution: I will make an app called CoinSnack that let Owen choose the snacks he want to order, store it, show him the receipt, and store that orders in the app’s database. While this Mini IA won’t solve the problem, it will provide the necessary codes that help solve the problem .Further codes in the future could then make it possible for the cafeteria’s staff to receive Owen’s snacks’ orders from the database. For now, with the limitation of time, I will just stop at the storing order part.

#### Sketches of Ideas

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4837.JPG)
Fig.1 Sketches of Coin Snack
/Description:

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4838.jpg)
Fig.2 Other sketches of Coin Snack
/Description:

## Criteria B: Design
#### System Diagram

![](https://github.com/BrightChanges/Unit-3/blob/main/CoinSnack%20System%20Diagram%20(1).png)
Fig. 3 System digram of CoinSnack app
/Descripton:

#### ER Diagram (DO THIS DIGITALLY AFTER THE WHOLE PROGRAM WORKED)

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4857.jpg)
Fig.4 ER Diagram of CoinSnack app's database
/Description: 


## Criteria C: Development
1st development story: The biggest struggle I got while I create this CoinSnack app is when I didn't know how to display text from python file (main.py) to the screen from kivy file (main.kv). I know I could easily solve this problem by going to Stack Overflow and ask, but then I know I can't do that every time as there is a limit for the amount of questions I can ask. So, I tried to not do that and see if I could go learn and solve the problem by myself. I took 2 hours everyday, for 5 days straight, searching and reading through the Internet, then I tried following those examples and coded. However, none of them worked until on the final day where I nearly gave up and decided to ask my Computer Science teacher tomorrow, I saw a Youtube video (https://www.youtube.com/watch?v=TEpHeuH7wNw&ab_channel=ErikSandberg) with the title "Kivy Core Concept - Using an `id` to Reference a Widget" and found the solution to solve my problem. If you look at the Youtube video's title, you can see that it is not related to text or anything at all. You can see that I cleary was desperate and was just watching every video I think could gave me some ideas to solve the problem. After I was able to solve this struggle, I learnt that I could solve any computer science/coding problem if I tried hard and put my mind into it.


#### Codes:

-main.py:

```.py

#Importing all the necessary libraries or widgets'codes
from kivy.lang import Builder
from sqlalchemy import Column, DateTime, String, Integer, ForeignKey
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship
from kivy.app import App
from kivy.uix.screenmanager import ScreenManager, Screen
from kivy.properties import StringProperty

from kivy.uix.behaviors import ButtonBehavior
from sqlalchemy.sql.functions import current_user

from kivymd.app import MDApp
from kivymd.uix.label import MDLabel
from kivymd.uix.picker import MDDatePicker
from kivymd.uix.screen import MDScreen
from kivymd.uix.menu import MDDropdownMenu
from kivy.uix.label import Label
from datetime import date
from kivymd.uix.list import OneLineListItem

#creating a base to connect with the database
Base = declarative_base()


#creating a screen manager for kivy
class ScreenManagement(ScreenManager):
    pass


#creating the table user in the database
class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, unique=True)
    email = Column(String(256), unique=True)
    password = Column(String)
    delivery_location = Column(String)


    #the relationship between the user table and the snack table is one-to-many:
    snacks = relationship("Snack", backref="user")


#creating the table snack in the database
class Snack(Base):
    __tablename__ = "snack"
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String)
    amount = Column(Integer)
    price = Column(Integer) #price in CoinSnack is in ¥
    user_id = Column(Integer, ForeignKey('user.id'))


# create the database and the connection
from sqlalchemy import create_engine

engine = create_engine('sqlite:///coinsnack.db')

from sqlalchemy.orm import sessionmaker

session = sessionmaker()
session.configure(bind=engine)
Base.metadata.create_all(engine)
##

#creating the backend code for Kivy's ImageScreen
class ImageScreen(MDScreen):
    
    #functions like one below could initiate some kind of actions to the app when 
    #they are called from buttons in the kivy file (main.kv).
    def go_back_to_order(self):
    
        #codes like one below switch screen for the app
        self.parent.current = "SnackScreen"


#creating the backend code for Kivy's MyAccountScreen
class MyAccountScreen(MDScreen):
    
    #initilization functions like the one below initialize items that could be resused in other function.
    #an extremely helpful function to stores data temporarily and let other function acesss them.
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.delivery_location = LoginScreen.delivery_location
        self.email = LoginScreen.email
    
    #this function on_pre_enter is a special kivy function that automatically
    #initiate a function once an user enter a screen.
    def on_pre_enter(self, *args):
        self.my_account()
    
    def my_account(self):
        
        #by taking the ids of the text from the kivy file, I could display the text from 
        #the backend python file (main.py) to the front end kivy screen (main.kv)
        self.ids.delivery_location.text = f"Your delivery location: {LoginScreen.delivery_location}"
        self.ids.email.text = f"Your email: {LoginScreen.email}"

    def home_page(self):
        self.parent.current = "HomeScreen"



#creating the backend code for Kivy's ThankyouScreen
class ThankyouScreen(MDScreen):

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.delivery_location = LoginScreen.delivery_location


    def on_pre_enter(self, *args):
        self.thankyou()

    def thankyou(self):
        #remove space from the LoginScreen.delivery_location (which is a string)
        LoginScreen.delivery_location.strip()
        self.ids.location_thankyou.text = f"Your order will arrived to your delivery location ( {LoginScreen.delivery_location}) in under 30 minutes."

    def home_page(self):
        self.parent.current = "HomeScreen"


#creating the backend code for Kivy's CheckoutScreen
class CheckoutScreen(MDScreen):

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.id = LoginScreen.current_user
        self.delivery_location = LoginScreen.delivery_location

    def on_pre_enter(self, *args):
        self.checkout()


    def checkout(self):
        #creating a blank string called order_list to stores the receipts information the user ordered
        order_list = ""
        #initialize the price variable so that I could used it later on.
        price = 0
        
        #creating connection to my database
        s = session()
        
        #a query command that query all the rows in the database table snack that have user_id equal to the user id of the current_id
        order_check = s.query(Snack).filter_by(user_id=LoginScreen.current_user).all()

        print(f"Delivery location is at {LoginScreen.delivery_location}")
        print(f"Total order of user with id {LoginScreen.delivery_location}:")
        
        #as order_check are multiple data rows, I need to use a for loop to get each variable of each row out for printing
        for i in range(len(order_check)):
            order_list += '\n' + "| No."+ str(i+1) + " | " + "Name: " + str(order_check[i].name).capitalize() + "| Amount: " + str(order_check[i].amount) + " | Price: " + str(order_check[i].price)+"¥ |"
            price += order_check[i].price
        print(order_list)


        self.ids.my_orders.text = order_list

        self.ids.delivery_location.text = f"Delivery location: {LoginScreen.delivery_location}"

        self.ids.total_price.text = f"Total cost: {price}¥"

    def confirm_order(self):
        print("Confirm button was clicked")
        self.parent.current = "ThankyouScreen"

    def go_back_to_order(self):
        self.parent.current = "SnackScreen"


#creating the backend code for Kivy's SnackScreen
class SnackScreen(MDScreen):

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.id = LoginScreen.current_user

    def see_snacks(self):
        self.parent.current = "ImageScreen"

    def check_out(self):
        print("Checkout button was pressed")
        self.parent.current = "CheckoutScreen"

    def add_to_cart(self):
        #initialize the pricce of each product
        price_per_product = 0
        print("Add to cart button was pressed")
        user_id = LoginScreen.current_user
        snack_name = self.ids.snack_name.text
        amount = self.ids.snack_amount.text
        
        #validation process codes to ensure that inputs form the user is valid:
        if snack_name != "Hamburger" and snack_name != "hamburger" and snack_name != "Coke" and snack_name != "coke" and snack_name!= "Popcorn"\
                and snack_name!="popcorn":

            print("Invalid food order")
        
        elif amount.isnumeric() == False:
            print("Invalid amount")

        else:

            if int(amount) > 50 or int(amount) < 1:
                print("The ordering amount is too much or too little.")
            else:
                # if user inputs are valid, further processed (including adding the data to the database) will be proceeded
                if (snack_name == "Hamburger" or snack_name ==  "hamburger"):
                    price_per_product = 300


                elif (snack_name == "Coke" or snack_name == "coke" or snack_name =="Popcorn" or snack_name == "popcorn"):
                    price_per_product = 100

                price = int(int(amount) * price_per_product)
                print(user_id, snack_name, int(amount), price)

                amount = int(amount)
                price = int(price)

                s = session()
                order = Snack(name=snack_name, amount=amount, price=price, user_id=user_id)
                s.add(order)
                s.commit()
                s.close()

                print("order from user with user id {}: snack_name: {} , amount: {}, price {} was added to database Snack".format(user_id, snack_name, amount, price))


    def home_page(self):
        self.parent.current = "HomeScreen"


#creating the backend code for Kivy's HomeScreen
class HomeScreen(MDScreen):

    def my_account(self):
        print("My account button clicked")
        self.parent.current = "MyAccountScreen"

    def order_snack(self):
        print("Order snacks (non-active one) clicked")
        self.parent.current = "SnackScreen"

    def log_out(self):
        self.parent.current = "LoginScreen"


class RegisterScreen(MDScreen):

    def try_register(self):

        print("Register was attempted")
        email = self.ids.email_input.text
        delivery_location = self.ids.delivery_location.text
        password_add = self.ids.password_input.text
        password_repeat = self.ids.password_check.text
        print(email, delivery_location, password_add, password_repeat)
        
        #checking if the user had input 2 same password
        if password_add == password_repeat:
            s = session()
            email = self.ids.email_input.text
            password = self.ids.password_input.text
            print(email, password)
            
            #checking if the registering email is already in the database or not
            email_check = s.query(User).filter_by(email=email).first()  ##similar to fetchone()

            if email_check:
                print("User already exists")
                s.close()
            
            #make sure that the input for the registering email address from the user is at an email
            elif "@" not in email:
                self.ids.email_input.error = True
                self.ids.email_input.helper_text = "Email is not valid"
                print("Email is invalid")
                
            else:
                #if the registering email is not already registered in the database,
                #the software will create the account for the user
                user = User(email=email, delivery_location=delivery_location, password=password)
                s.add(user)

                s.commit()
                s.close()

                self.parent.current = "LoginScreen"

        else:
            print("The passwords do not match")

    def switch_to_login(self):
        self.parent.current = "LoginScreen"


#a class for a ButtonLabel in the kifvy file (main.kv)
class ButtonLabel(ButtonBehavior, MDLabel):
    pass


#creating the backend code for Kivy's LoginScreen
class LoginScreen(MDScreen):
    # all 3 codes below set up variables that store user information after the user logged in 
    #through the LoginScreen
    current_user = None 
    delivery_location = None
    email = None

    def try_login(self):
        email = self.ids.email_input.text
        password = self.ids.password_input.text
        print(email,password)
        s = session()
        user_check = s.query(User).filter_by(email=email, password=password).first()
        print(user_check)

        if user_check:
            s.close()
            print(f"login successful for user with email '{email}")
            LoginScreen.current_user = user_check.id  # getting the id of the current user
            LoginScreen.delivery_location = user_check.delivery_location  # getting the deliver location of the current user
            LoginScreen.email = user_check.email #getting the email of the current user
            self.parent.current = "HomeScreen"

        else:
            print("User does not exist or wrong password/email")


#creating the backend code for Kivy's whole app
class MainApp(MDApp):

    def build(self):
        self.theme_cls.primary_palette = "Indigo"


#The code that will run the whole Kivy's app
MainApp().run()







```

-main.kv:

```.py
#creating the screen manager for multiple screens
ScreenManager:
    id: scr_manager

    LoginScreen:
        name: "LoginScreen"

    RegisterScreen:
        name: "RegisterScreen"

    HomeScreen:
        name: "HomeScreen"

    SnackScreen:
        name: "SnackScreen"

    CheckoutScreen:
        name: "CheckoutScreen"

    ThankyouScreen:
        name: "ThankyouScreen"

    MyAccountScreen:
        name: "MyAccountScreen"

    ImageScreen:
        name: "ImageScreen"


#creating the title for the image
<MyTile@SmartTileWithLabel>
    size_hint_y: None
    height: "240dp"


#The screen for the food images
<ImageScreen>


    MDBoxLayout:
        id: logo_box
        adaptive_height: True
        orientation: "vertical"


        Image:
            source: "CoinSnack_Logo.png"


    ScrollView:

        #codeds to show the image grids
        MDGridLayout:
            cols: 3
            adaptive_height: True
            padding: dp(4), dp(4)
            spacing: dp(4)

            MyTile:
                source: "hamburger1.jpeg"
                text: "[size=26]Hamburger [/size]\n[size=14]¥300[/size]"
                tile_text_color: app.theme_cls.accent_color


            MyTile:
                source: "coke.jpg"
                text: "[size=26]Coke [/size]\n[size=14]¥100[/size]"
                tile_text_color: app.theme_cls.accent_color

            MyTile:
                source: "popcorn.jpg"
                text: "[size=26]Popcorn  [/size]\n[size=14]¥100[/size]"
                tile_text_color: app.theme_cls.accent_color


    MDRectangleFlatIconButton:
        icon: "backburger"
        text: "Return to ordering"
        halign: "center"
        on_release:
            root.go_back_to_order()







<MyAccountScreen>
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "coke_snack.jpeg"


    MDCard:
        size_hint: 0.5, 0.95
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"


        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)


            MDBoxLayout:
                id: logo_box
                adaptive_height: True
                orientation: "vertical"


                Image:
                    source: "CoinSnack_Logo.png"

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:
                text: "Account information:"
                font_style: "H4"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:
                id: email
                font_style: "Subtitle2"
                halign: "center"

            MDLabel:
                id: delivery_location
                font_style: "Subtitle2"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDRectangleFlatIconButton:
                icon: "home"
                text: "Return to homepage"
                halign: "center"
                on_release:
                    root.home_page()


#the screen for the thank you page
<ThankyouScreen>
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "coke_snack.jpeg"


    MDCard:
        size_hint: 0.5, 0.95
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"


        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)


            MDBoxLayout:
                id: logo_box
                adaptive_height: True
                orientation: "vertical"


                Image:
                    source: "CoinSnack_Logo.png"

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:
                text: "Thank you for your order!"
                font_style: "H4"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:
                id : location_thankyou
                font_style: "H6"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:
                text: "Thank you for using CoinSnack. We hope you will have a great time with your snacks. "
                font_style: "Subtitle2"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:

            MDRectangleFlatIconButton:
                icon: "home"
                text: "Return to homepage"
                halign: "center"
                pos_hint: {'center_x': .3, 'center_y': .3}
                on_release:
                    root.home_page()

            MDLabel:

            MDLabel:


#the screen for the checkout screen
<CheckoutScreen>
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "coke_snack.jpeg"


    MDCard:
        size_hint: 0.5, 0.95
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"



        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)

            MDBoxLayout:
                id: logo_box
                adaptive_height: True
                orientation: "vertical"


                Image:
                    source: "CoinSnack_Logo.png"

            MDLabel:
                text: "Your order"
                font_style: "H4"
                halign: "center"




            MDLabel:

            MDLabel:
                text: "Receipts:"
                font_style: "H5"
                halign: "center"

            MDLabel:

            MDLabel:
                id: my_orders
                font_style: "Subtitle2"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:

            MDLabel:
                id: total_price
                font_style: "Subtitle1"
                halign: "center"


            MDLabel:



            MDLabel:
                text: "Payment: Cash upon delivery"
                font_style: "Subtitle1"
                halign: "center"


            MDLabel:
                id: delivery_location
                font_style: "Subtitle1"
                halign: "center"

            MDLabel:

            MDLabel:

            MDLabel:



            MDRectangleFlatIconButton:
                icon: "cart-check"
                text: "Confirm order"
                halign: "center"
                on_release:
                    root.confirm_order()

            MDRectangleFlatIconButton:
                icon: "cart-arrow-down"
                text: "Return to keep ordering"
                halign: "center"
                on_release:
                    root.go_back_to_order()







# the screen to order snacks
<SnackScreen>:

    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "coke_snack.jpeg"

    MDCard:
        size_hint: 0.5, 1
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)


            MDBoxLayout:
                id: logo_box
                adaptive_height: True
                orientation: "vertical"

                Image:
                    source: "CoinSnack_Logo.png"


            MDLabel:
                text: "Order your snack:"
                font_style: "H6"
                halign: "center"


            MDRectangleFlatIconButton:
                icon: "food"
                text: "See image of the snacks"
                pos_hint: {'x': .18}
                on_release:
                    root.see_snacks()


            MDLabel:

            MDLabel:
                font_style: "Body2"
                text: "We sell Hamburger, Coke, and Popcorn. Their price are: Hamburger: ¥300, Coke: ¥100, Popcorn: ¥100. To order, please type in their name down below:"
                halign: "center"



            MDLabel:

            MDLabel:

            MDLabel:
                font_style: "Body2"
                text: "Please add each food each time to the cart. Once you added all the food you want to order to the cart, please click the check out button."
                halign: "center"

            MDLabel:

            MDTextField:
                id:snack_name
                hint_text: "Name of the snack"
                icon_right: "food"
                helper_text: "Invalid food"
                helper_text_mode: "on_error"
                required: True

            MDTextField:
                id:snack_amount
                hint_text: "Amount of snack you want to order:"
                icon_right: "account-question"
                helper_text: "Invalid number"
                helper_text_mode: "on_error"
                required: True

            MDBoxLayout:
                id: button
                adaptive_height: True
                orientation: "horizontal"
                padding: dp(10)
                spacing: dp(10)

                MDRectangleFlatIconButton:
                    icon: "cart-arrow-down"
                    text: "Add to cart"
                    halign: "center"
                    pos_hint: {'center_x': .1, 'center_y': .3}
                    on_release:
                        root.add_to_cart()


                MDRectangleFlatIconButton:
                    icon: "cart-arrow-right"
                    text: "Check out"
                    halign: "center"
                    pos_hint: {'center_x': .2, 'center_y': .3}
                    on_release:
                        root.check_out()

            MDRectangleFlatIconButton:
                icon: "home"
                text: "Return to homepage"
                halign: "center"
                pos_hint: {'center_x': .3, 'center_y': .3}
                on_release:
                    root.home_page()



#the home screen
<HomeScreen>:
#this is for the visual
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "coke_snack.jpeg"

    MDCard:
        size_hint: 0.5, 0.75
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"



        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)

            MDBoxLayout:
                id: logo_box
                adaptive_height: True
                orientation: "vertical"


                Image:
                    source: "CoinSnack_Logo.png"

            MDLabel:

            MDRectangleFlatIconButton:
                icon: "account-circle"
                pos_hint: {'x': .315}
                text: "My Account"
                on_release:
                    root.my_account()

            MDRectangleFlatIconButton:
                icon: "food"
                pos_hint: {'x': .305}
                text: "Order snacks"
                on_release:
                    root.order_snack()

            MDRectangleFlatIconButton:
                icon: "logout"
                pos_hint: {'x': .340}
                text: "Log out"
                on_release:
                    root.log_out()

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
            source: "coke_snack.jpeg"

    MDCard:
        size_hint: 0.5, 1
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"




        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)


            MDBoxLayout:
                id: logo_box
                adaptive_height: True
                orientation: "vertical"


                Image:
                    source: "CoinSnack_Logo.png"




            MDLabel:
                text: "Register"
                font_style: "H6"
                halign: "center"

            MDLabel:

            MDTextField:
                id: email_input
                hint_text: "Email"
                icon_left: "email"
                helper_text: "Invalid email"
                helper_text_mode: "on_error"
                required: True

            MDTextField:
                id: delivery_location
                hint_text: "Delivery location"
                icon_left: "location"
                helper_text: "Invalid location"
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

            MDRectangleFlatIconButton:
                icon: "food"
                text: "Register"
                on_release:
                    root.try_register()

            MDRectangleFlatIconButton:
                icon: "account-key"
                text: "Log in"
                on_release:
                    root.parent.current = "LoginScreen"




#The LoginScreen
<LoginScreen>:
    BoxLayout:
        orientation: 'vertical'
        size: root.height, root.width

        FitImage:
            source: "coke_snack.jpeg"

    MDCard:
        size_hint: 0.5, 0.75
        elevation: 10
        pos_hint: {"center_x": 0.5, "center_y": 0.5}
        orientation: "vertical"



        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: "vertical"
            padding: dp(30)
            spacing: dp(20)



            MDLabel:

            MDLabel:

            MDBoxLayout:
                id: logo_box
                adaptive_height: True
                orientation: "vertical"


                Image:
                    source: "CoinSnack_Logo.png"





            MDLabel:
                text: "Login"
                font_style: "H6"
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

            MDRectangleFlatIconButton:
                icon: "account-key"
                text: "Log in"
                on_release:
                    root.try_login()

            MDBoxLayout:
                adaptive_height: True
                halign: "right"

                MDRectangleFlatIconButton:
                    icon: "food"
                    text: "Register"
                    on_press:
                        print("here")
                        root.parent.current = "RegisterScreen"










```

2st development story: Another struggle I encoutered when developing CoinSnack was how to display an image on the screen. It took me hours of doing trials and errors, researching to finally found out that in order to display an image from the kivy file (main.kv), I need to put that image label inside a layout label such as MDBoxLayout. While I could solve this problem quicker than the problem in 1st development story, this problem still took me a lot of time. However, same as the previous problem, after I was able to solve this problem, I even believe stronger that I could solve any coding problems if I commit myself into finding its solution.


## Criteria D: Functionality
#### Alpha Testing (following the Unit Testing, Code Review, Integration testing success criteria tables):
Link to the desmontration video:...

#### Beta Testing with colleagues (following the Software testing success criteria table):
Link to the desmontration video:...

#### Acceptance Testing with client (following User acceptance testing table):
Link to the desmontration video:...

...
Fig. Picture 1 of CoinSnack /Description:

...
Fig. Picture 2 of CoinSnack /Description:

...
Fig. Picture 3 of CoinSnack /Description:



## Criteria E: Evaluation
#### Procedures in the success criteria table need to be very specific. It is one of the most important aspect for the grade of Criteria E.
#### Success Criteria for Alpha Testing (combination of Unit Testing, Code Review, Integration testing) (!IN PROGESS!)


| Criteria                                                                                                                                                                                  | Expected outcome                                                                                                                                                                                                                                                                                                   | Met? |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------|
| Criteria 1: Client can register an account and store inputs  such as Email, Delivery Location, Password when using an email address that is not already registered.                       | A client can register an account by running the program, click on "Register"button in the LoginScreen, fill in all the inputs (Email, Delivery Location, Password) and click the "Register" button in the RegisterScreen, with the condition that the Email the user input is not already registered in CoinSnack. |      |
| Criteria 2: The program will print "Email is invalid" on to the  terminal if the client registered an invalid email address.                                                              | When a client registers an email address that is not an email address (such as putting in something without a "@"), CoinSnack app will print "Email is invalid" on to the screen, and not let the client register their CoinSnack account.                                                                         |      |
| Criteria 3: The program can validate registered client and let them to log in and use the app.                                                                                            | When a registered client type in all the necessary inputs such as Email and Password, and click the "Log in" button, the program will log the user in,  bringing them into the HomeScreen of the app.                                                                                                              |      |
| Criteria 4: The program will print "User does not exist or wrong  password/email" on to the terminal if the client entered a  non-registered email/wrong email or password.               | When an unregistered client typed in their email, or when a registered client typed in the wrong email/password, CoinSnack app will print "User does not  exist or wrong password/email" on to the terminal and not let the client to log in to the HomeScreen.                                                    |      |
| Criteria 5: The client can see their account information (including their information about their Email, Delivery location) by clicking on the "My Account" button in the HomeScreen.     | After the client is logged in and clicking the "My Account" button, they will be redirected to the MyAccountScreen, where their informations (Email,  Delivery location) will be displayed.                                                                                                                        |      |
| Criteria 6: The client is moved to the SnackOrderingScreen when clicking the "Order snacks" button in the HomeScreen.                                                                     | When the logged in client click on the "Order snacks" button in the  HomeScreen, they will be redirected to the SnackScreen, where he or she could start choosing and ordering snacks.                                                                                                                             |      |
| Criteria 7: The client is moved to the SnackImageScreen when  clicking the "See images of the snacks", where he or she will see  pictures of all the snacks they can order from CoinSnack | When the client clicks on the "See images of the snacks" button in the  SnackOrderingScreen, he or she will be moved to the SnackImageScreen,  where he or she will see pictures of all the snacks they can order from CoinSnack                                                                                   |      |
