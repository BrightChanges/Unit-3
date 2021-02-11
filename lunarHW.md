## 1.Draw the ER diagram:
![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4730.jpg)

## 2.Create the SQL script to create the database and add some sample users:

```.py

CREATE TABLE IF NOT EXISTS User (
	id integer,
	email varchar(100),
	username varchar(100),
	password varchar(250),
    PRIMARY KEY (id,email)
);


CREATE TABLE IF NOT EXISTS Orders(
    id integer PRIMARY KEY,
    user_id integer,
    order_id integer,
    FOREIGN KEY(user_id) REFERENCES  User(id),
    FOREIGN KEY(order_id) REFERENCES  Snack(id)
);


CREATE TABLE IF NOT EXISTS Snack (
	id integer PRIMARY KEY AUTOINCREMENT NOT NULL,
	name text,
	amount int
);

INSERT INTO User
VALUES
(1,'2022.kien.letrung@uwcisak.jp', 'Kien Le Trung','isak123'),
(2, '2022.owenchen@uwcisak.jp', 'Owen Chen', 'rice256'),
(3, 'junpei@gmail.com', 'Junpei', 'riseyeah');


```

## 3.Create the UML class diagram for the db
![](https://github.com/BrightChanges/Unit-3/blob/main/IMG_4731.jpg)


## 4.Create the database using ORM and add some sample users

-Python codes:

```.py

from sqlalchemy import Column, DateTime, String, Integer, ForeignKey
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship

Base = declarative_base()

class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True)
    email = Column(String(100), primary_key=True)
    username = Column(String(100))
    password = Column(String(250))
    #relationship "writes in the ER diagram" one-to-many
    posts = relationship("Snack", backref="customer") #Post is referencing to the class Post below

    def __repr__(self):
        return f"<User with username {self.username} with email {self.email}>"

class Snack(Base):
    __tablename__ = "snack"
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey('user.id'))
    name = Column(String)
    amount = Column(Integer)

from sqlalchemy import create_engine
engine = create_engine('sqlite:///ORM_HW')

from sqlalchemy.orm import sessionmaker
session = sessionmaker()
session.configure(bind=engine)
Base.metadata.create_all(engine)


s = session()

## Add some users into the User table

user1 = User(id=1,email="2022.kien.letrung@uwcisak.jp",username='Kien Le Trung', password='isak123')
user2 = User(id=2,email="2022.owenchen@uwcisak.jp",username='Owen Chen', password='rice256')
user3 = User(id=3,email="junpei@gmail.com",username='Junpei', password='riseyeah')
s.add(user3)
s.commit()

s.close()

```
-Image of the Python codes:

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-02-11%20at%201.47.28%20PM.png)

-Image1 of the database created by this ORM-Python codes (showing the database structure):

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-02-11%20at%201.47.52%20PM.png)

-Image2 of the database created by this ORM-Python codes (showing the sample users):

![](https://github.com/BrightChanges/Unit-3/blob/main/Screen%20Shot%200003-02-11%20at%201.48.08%20PM.png)

## 5.Research what Hash functions are and think how they could be used to improve the security of the private data in the database



