# OIBSIP-TASK1
# Code explanation:

# Login/register

1. The user should either log in or register.
2.  If the user is new to this booking system, then the user must register. Otherwise, they can choose to log in. 
3. In the login process, the user's name and password are checked with the database and table named user details .
4. If the user has already registered, a 'welcome' message is shown; otherwise, the user needs to do register.
5. when the user enters username and password this details checked with database verification process done.

# From to location details:

1. After this, the names of the from and to location  will be displayed. The user has to choose one of the location.
2. The location details fetched from Database table and displayed to users.

# Train details:

1.Based on the from, to location choice, the list of trains will be displayed.
2. The trains details fetched from database and displayed to user based on the location input given by the user.

# Choosing train:

The user has to choose a train from the list displayed.

# Seating details

1.There is a separate seating layout for each train .
2. In the seating layout, the available seats and booked seats are displayed.
3.  'FU' represents booked seats, so the user has to choose other seats only.
4. The no of required by the user is already collected and based on the seat requirement the user allowed to choose the seats.
5. The user also need to choose the class for booking tickets ( fisrt class or second class ).

 # Ticket booking details:

Finally, the ticket details will be displayed. 
From :  location
To : location
Train name and ID: 
Booking date & time: 
No of Admits: 
Seats:
Date of journey:
Price of one ticket:
# ( for first class 250 rs and for second class 150 rs ( varies based on class selected by user )).
Total price: 

THANKS FOR BOOKING TICKETS.

Message will be displayed at last. 


User details and ticket booking details are maintained in separate tables in the DB.

# To run the code make use of following commands

# To compile
javac filename.java
# To execute 
java -cp .;connectorname.jar ClassName
