# CoinSnack | Mini-IA
### Created by Kien Le Trung


## Criteria A: Planning
### Context of the product
My client is Owen, a student in my school. In conversation with Owen (see appendix A), he says that at night, he couldn't buy snacks at the cafeteria and he would love someone could provide him the opportunity to buy snacks easily at night. 

Link to appendix A: ![](https://github.com/BrightChanges/Unit-3/blob/main/CS%20Mini%20IA%20interview.m4a)

From this interview with Owen, I decided to create an app that lets Owen order snacks at night called CoinSnack.
It will let Owen choose and order snacks, printing the receipts and stores his order in the app's database, where the data could possibily be sent to the delivery person and the snack company. (In this Mini-IA, I will not demonstrate how the data is sent to the delivery person.)

#### Sketches of Ideas

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4837.JPG)
Fig.1 Sketches of Coin Snack

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4838.jpg)
Fig.2 Other sketches of Coin Snack

## Criteria B: Design
#### System Diagram

![](https://github.com/BrightChanges/Unit-3/blob/main/CoinSnack%20System%20Diagram.png)

#### ER Diagram (DO THIS DIGITALLY AFTER THE WHOLE PROGRAM WORKED)

![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4857.jpg)
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

Base = declarative_base()


class ScreenManagement(ScreenManager):
    pass


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, unique=True)
    email = Column(String(256), unique=True)
    password = Column(String)
    delivery_location = Column(String)


    # relationship "has in the ER diagram" one-to-many
    snacks = relationship("Snack", backref="user")


#Need to update the ER Diagram on Github!
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


class ImageScreen(MDScreen):

    def go_back_to_order(self):
        self.parent.current = "SnackScreen"



class MyAccountScreen(MDScreen):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.delivery_location = LoginScreen.delivery_location
        self.email = LoginScreen.email

    def on_pre_enter(self, *args):
        self.my_account()

    def my_account(self):
        self.ids.delivery_location.text = f"Your delivery location: {LoginScreen.delivery_location}"
        self.ids.email.text = f"Your email: {LoginScreen.email}"

    def home_page(self):
        self.parent.current = "HomeScreen"




class ThankyouScreen(MDScreen):

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.delivery_location = LoginScreen.delivery_location


    def on_pre_enter(self, *args):
        self.thankyou()

    def thankyou(self):
        LoginScreen.delivery_location.strip()
        self.ids.location_thankyou.text = f"Your order will arrived to your delivery location ( {LoginScreen.delivery_location}) in under 30 minutes."

    def home_page(self):
        self.parent.current = "HomeScreen"



class CheckoutScreen(MDScreen):

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.id = LoginScreen.current_user
        self.delivery_location = LoginScreen.delivery_location

    def on_pre_enter(self, *args):
        self.checkout()


    def checkout(self):
        order_list = ""
        price = 0
        s = session()
        order_check = s.query(Snack).filter_by(user_id=LoginScreen.current_user).all()

        print(f"Delivery location is at {LoginScreen.delivery_location}")
        print(f"Total order of user with id {LoginScreen.delivery_location}:")
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

class SnackScreen(MDScreen):

    # string123 = StringProperty("")

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.id = LoginScreen.current_user

    def see_snacks(self):
        self.parent.current = "ImageScreen"

    def check_out(self):
        print("Checkout button was pressed")
        self.parent.current = "CheckoutScreen"

    def add_to_cart(self):
        price_per_product = 0
        print("Add to cart button was pressed")
        # print(LoginScreen.current_user) #LoginScreen.current_user is the id of the user
        user_id = LoginScreen.current_user
        snack_name = self.ids.snack_name.text
        amount = self.ids.snack_amount.text

        if snack_name != "Hamburger" and snack_name != "hamburger" and snack_name != "Coke" and snack_name != "coke" and snack_name!= "Popcorn"\
                and snack_name!="popcorn":

            print("Invalid food order")

        elif amount.isnumeric() == False:
            print("Invalid amount")

        else:

            if int(amount) > 50 or int(amount) < 1:
                print("The ordering amount is too much or too little.")
            else:
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








 #######The code below is for the dropdown, is not working so currently stop developing:
    #     menu_items = [{"text": "Hamburger"}, {"text": "Noodle"} ]
    #
    #     self.menu = MDDropdownMenu(
    #         #THIS CODE BELOW IS CAUSING ERROR!:
    #         # caller = self.ids.dropdown_item,
    #         ######################################
    #         items = menu_items,
    #         position = "center",
    #         width_mult=4,
    #     )
    #     self.menu.bind(on_release=self.set_item)
    #
    # value_hold = 0
    #
    # def set_item(self, instance_menu, instance_menu_item):
    #     self.ids.dropdown_item.set_item(instance_menu_item.text)
    #     instance_menu.dismiss()
    #



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

        if password_add == password_repeat:
            s = session()
            email = self.ids.email_input.text
            password = self.ids.password_input.text
            print(email, password)

            email_check = s.query(User).filter_by(email=email).first()  ##similar to fetchone()

            if email_check:
                print("User already exists")
                s.close()

            else:
                user = User(email=email, delivery_location=delivery_location, password=password)
                s.add(user)

                s.commit()
                s.close()

                self.parent.current = "LoginScreen"

        else:
            print("The passwords do not match")

    def switch_to_login(self):
        self.parent.current = "LoginScreen"


class ButtonLabel(ButtonBehavior, MDLabel):
    pass


class LoginScreen(MDScreen):
    current_user = None
    delivery_location = None
    email = None

    def try_login(self):
        email = self.ids.email_input.text
        password = self.ids.password_input.text
        print(email,password)
        s = session()
        # THIS QUERY CODE BELOW only work if I add .first()=>ask Dr Ruben why so?
        user_check = s.query(User).filter_by(email=email, password=password).first()
        print(user_check)

        if user_check:
            s.close()
            print(f"login successful for user with email '{email}")
            LoginScreen.current_user = user_check.id  # getting the ide of the current user
            LoginScreen.delivery_location = user_check.delivery_location  # getting the deliver location of the current user
            LoginScreen.email = user_check.email #getting the email of the user
            self.parent.current = "HomeScreen"

        else:
            print("User does not exist")


class MainApp(MDApp):

    def build(self):
        self.theme_cls.primary_palette = "Indigo"



MainApp().run()







```

-main.kv:

```.py
#:import toast kivymd.toast.toast

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


<MyTile@SmartTileWithLabel>
    size_hint_y: None
    height: "240dp"



<ImageScreen>


    MDBoxLayout:
        id: logo_box
        adaptive_height: True
        orientation: "vertical"


        Image:
            source: "CoinSnack_Logo.png"


    ScrollView:

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

#How to change the color of background of a MDCard

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

    ###Trying to create a swiper:
            #MDSwiper:
                #size_hint_y: None
                #height: root.height
                #y: root.height - self.height
                #on_swipe: self.get_current_item().ids.icon.shake()

                #MySwiper:

                #MySwiper1:
    ###

    ###Trying to create a dropdown:
            #MDDropDownItem:
                #id: dropdown_item
                #text: 'Hamburger'
                #pos_hint: {'center_x': .5, 'center_y': .6}
                #current_item: 'Hamburger'
                #on_release: root.menu.open()

            #MDRaisedButton:
                #pos_hint: {'center_x': .5, 'center_y': .3}
                #text: 'Check Item'
                #on_release: toast(dropdown_item.current_item)
    ###
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

#How to change the c0lor of background of a MDCard

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

#How to change the c0lor of background of a MDCard


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


#How to change the c0lor of background of a MDCard

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

2st development story:


## Criteria D: Functionality
#### Alpha Testing:
Link to the desmontration video:...

#### Beta Testing with colleagues:
Link to the desmontration video:...

#### Acceptance Testing with client:
Link to the desmontration video:...

...
Fig. Picture 1 of CoinSnack

...
Fig. Picture 2 of CoinSnack

...
Fig. Picture 3 of CoinSnack 



## Criteria E: Evaluation
##### Success Criteria
...

