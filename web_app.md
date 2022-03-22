## Feedback web app - Python, Flask, Postgres and Heroku

Our, app will also be sending email using mailtrap

## Start 

1. Create a empty project folder in vs code.
2. On the terminal-
	`pip install pipenv`
	
	This will create a new virtual environment and initialize it. This is to manage package dependency of our web app. Use `exit` to exit from environment.
	
	`pipenv shell`
	
	This command will create a pipfile in default config in our empty project directory, now as we add packages, those will get reflected in pipfile.
	
	`pipenv install flask`
	`pipenv install psycopg2`
	`pipenv install psycopg2-binary`
	`pipenv install flask-sqlalchemy`
	`pipenv install gunicorn`  -> To start http - server, required while dealing with Heroku
	
	Now, select correct intepreter in vs code - to pipenv interpreter
	
3. Create 2 empty folders, with names-
	static - this will have css files, image logo
	
	Copy the css style sheet and logo from github repo.
	
	templates - 2 html templates file - index.html, success.html
	
4. Start working on index.html-

	We are creating our webapp of 2 pages only, so first page is index.html where the customer will give his feedback and second is success.html which shows successful submission.
	
	In index.html load the html-boilerplate code and make changes, to create front-end of our app.
	
5. Create app.py file to start dealing with the backend.

### FLOW OF CODE STRUCTURE-

- import libraries - flask, request and render_template
- create flask app 
- create 2 endpoints with app.route(): 1st endpoint will load the empty index.html and 2nd will submit endpoint.
- The submit endpoint will be loaded after the user clicks on submit button in index.html and passes certain conditions.
- Since, the customer will sending the data to the web server, this request type will be post.
- In submit function: we will get the field values from request.form, why form? because it is the class name of fields in index.html document. It is like get me 'customer' field of request.form class --> written as `request.form['Customer']`
- in both the endpoint we return with render_template method, that loads the html file.
- 
- **Adding Database to the functionality**
-
- import sql_alchemy and create db object which is instance of sql_alchemy for our app, written as `db = SQLALCHEMY(app)`
- now create a datamodel for our customer feedback data. The customer feedback will have 4 fields - id -> autoincrementing primary key, customer name, dealer name, rating for service and comments which is text.
- remember when creating data model to inherit from db.Model class. Specify column type for all fields. And also, create a `__init__` constructor method.
- with a simple, if-else statement handle case when running app in production or in develop
when running in dev set debug to True and SQLALCHEMY_DATABASE_URI to local postgres. when running from heroku set it to heroku postgres database address.
- We can add a function `send_mail()` that sends confirmation mail of the feedback to the customer. We used following libraries for it.
	
	`import smtplib`
		
	`from email.mime.text import MIMEText`
	
	We start a SMTP server on `smtp.mailtrap.io` at a set port number, we login to the server and execute a sendmail method with the sender-reciever and message details.
	
Finally, deploy the code to heroku
	





