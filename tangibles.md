{% include navigation.html %}

Wireframe

[Wireframe](https://www.figma.com/file/GR7a3HWdAPiNaj1hV8ryXD/Wireframe-Tri-3?node-id=0%3A1)

[Brainwriting](https://docs.google.com/document/d/17YyLoAyvybri-im0QEUe5gXrXrxKp6mghYwbyYo1NWk/edit?usp=sharing)

## PBL Final Project Documentation

### Initial Weeks

-Planning, Brainstorming, Finalizing features
  -Budget Tracker, Grocery List, Map, Calendar, To-Do list
  
### Budget Tracker Table

- Made a budget tracker with form entires for different money values, calculates the monthly and weekly total at end

```
   function total() {
        var num1 = document.budgetForm.rent.value;
        var num2 = document.budgetForm.car.value;
        var num3 = document.budgetForm.phone.value;
        var num4 = document.budgetForm.foodGas.value;
        var num5 = document.budgetForm.other.value;

        if (isNaN(num1) || isNaN(num2) || isNaN(num3) || isNaN(num4) || isNaN(num5)) {
            alert('*** Sorry, you can only budget numbers. ***');
            total = "$0.00";
        } else if (num1 < 1 && num2 < 1 && num3 < 1 && num4 < 1 && num5 < 1) {
            total = "$0.00";
        } else {
            var total = parseInt(num1) + parseInt(num2) + parseInt(num3) + parseInt(num4) + parseInt(num5);
            document.getElementById("answer").value = '$' + total / 4 + '.00';
            document.getElementById("answer2").value = '$' + total + '.00';
        }
    }
  ```
  
  ### Tutoring Help Database

  -CRUD Database that contains names, emails, phone number and subject that each tutor specializes in
  -Makes it easy for college students to find tutors to help them in classes
  
  ```
  <tr>
                        <th><label for="userid">User ID</label></th>
                        <th><label for="name">Name</label></th>
                        <th><label for="email">Email</label></th>
                        <th><label for="subject">Subject</label></th>
                        <th><label for="phone">Phone</label></th>
                    </tr>
  
  
  
  ```
  
  1st Table
  
  ```
  class Users(UserMixin, db.Model):
    __tablename__ = 'users'

    # Define the Users schema
    userID = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(255), unique=False, nullable=False)
    email = db.Column(db.String(255), unique=True, nullable=False)
    password = db.Column(db.String(255), unique=False, nullable=False)
    phone = db.Column(db.String(255), unique=False, nullable=False)
    # Defines a relationship between User record and Notes table, one-to-many (one user to many notes)
    notes = db.relationship("Notes", cascade='all, delete', backref='users', lazy=True)

    # constructor of a User object, initializes of instance variables within object
    def __init__(self, name, email, password, phone):
        self.name = name
        self.email = email
        self.set_password(password)
        self.phone = phone

    # returns a string representation of object, similar to java toString()
    def __repr__(self):
        return "Users(" + str(self.userID) + "," + self.name + "," + str(self.email) + ")"

    # CRUD create/add a new record to the table
    # returns self or None on error
    def create(self):
        try:
            # creates a person object from Users(db.Model) class, passes initializers
            db.session.add(self)  # add prepares to persist person object to Users table
            db.session.commit()  # SqlAlchemy "unit of work pattern" requires a manual commit
            return self
        except IntegrityError:
            db.session.remove()
            return None

    # CRUD read converts self to dictionary
    # returns dictionary
    def read(self):
        return {
            "userID": self.userID,
            "name": self.name,
            "email": self.email,
            "password": self.password,
            "phone": self.phone,
            "notes": self.notes,
            "query": "by_alc"  # This is for fun, a little watermark
        }

    def read2(self):
        return {
            "userID": self.userID,
            "name": self.name,
            "email": self.email,
            "phone": self.phone,

        }

    # CRUD update: updates users name, password, phone
    # returns self
    def update(self, name="", email="", password="", phone=""):
        """only updates values with length"""
        if len(name) > 0:
            self.name = name
        if len(email) > 0:
            self.email = email
        if len(password) > 0:
            self.set_password(password)
        if len(phone) > 0:
            self.phone = phone
        db.session.commit()
        return self

    # CRUD delete: remove self
    # None
    def delete(self):
        db.session.delete(self)
        db.session.commit()
        return None

 

  ```
   
   
   Notes Tables
   
  ```
  class Notes(db.Model):
    __tablename__ = 'notes'

    # Define the Notes schema
    id = db.Column(db.Integer, primary_key=True)
    note = db.Column(db.Text, unique=False, nullable=False)
    image = db.Column(db.String, unique=False)
    # Define a relationship in Notes Schema to userID who originates the note, many-to-one (many notes to one user)
    userID = db.Column(db.Integer, db.ForeignKey('users.userID'))

    # Constructor of a Notes object, initializes of instance variables within object
    def __init__(self, userID, note, image):
        self.userID = userID
        self.note = note
        self.image = image

    # Returns a string representation of the Notes object, similar to java toString()
    # returns string
    def __repr__(self):
        return "Notes(" + str(self.id) + "," + self.note + "," + str(self.userID) + ")"

    # CRUD create, adds a new record to the Notes table
    # returns the object added or None in case of an error
    def create(self):
        try:
            # creates a Notes object from Notes(db.Model) class, passes initializers
            db.session.add(self)  # add prepares to persist person object to Notes table
            db.session.commit()  # SqlAlchemy "unit of work pattern" requires a manual commit
            return self
        except IntegrityError:
            db.session.remove()
            return None

    # CRUD read, returns dictionary representation of Notes object
    # returns dictionary
    def read(self):
        return {
            "id": self.id,
            "userID": self.userID,
            "note": self.note,
            "image": self.image
        }


# Define the Users table within the model

  ```


