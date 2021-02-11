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

