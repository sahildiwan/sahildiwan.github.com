---
layout: post
title:  "Making a Flask app using a PostgreSQL database and deploying to Heroku"
date:   2014-07-17 12:08:00
categories: jekyll update
---

This blog post is about creating a simple pre-registration page using the best (in my opinion) micro web-development framework for Python, [Flask][1]. We will be connecting our pre-registation app to a [PostgreSQL][2] database locally and in the cloud. Once we have our Flask app running locally, I will show you how to successfully deploy it to [Heroku][3].

Notes: 

- The source code for this project can be found [here][4].
- A live demo of this project can be found on Heroku [here](http://flask-postgres-heroku.herokuapp.com/).
- I am running Mac OS X and this tutorial (at least installation) will be specific.

Let's get started.

`1.)` Installation and environment setup

We will need to install the following:

- [pip][5]
- [virtualenv][6]
- [postgres.app][7]
- And obviously [Flask][1] + [Flask-SQLAlchemy](https://pythonhosted.org/Flask-SQLAlchemy/)

First, install pip by running the command

{% highlight bash %}
$ sudo easy_install pip
{% endhighlight %}

in your terminal.

Then, install virtualenv by running the command

{% highlight bash %}
$ sudo pip install virtualenv
{% endhighlight %}

Now, we can create our project directory and set up our virtual environment.

{% highlight bash %}
$ cd your/fav/directory
$ mkdir lovelypreregpage
{% endhighlight %}

To set up a virtual environment

{% highlight bash %}
$ cd lovelypreregpage
$ virtualenv venv
{% endhighlight %}

You should now have a `venv` folder within your `lovelypreregpage` project.

To start your virtual environment run the command

{% highlight bash %}
$ . venv/bin/activate
{% endhighlight %}

Next, we can begin setting up our database. Go to [postgresapp.com][7] and download/install `postgres.app` just like any other application. `Postgres.app` is super easy to set up, but don't forget to add the following to your `.bash_profile` found in your home directory

{% highlight bash %}
# Postgres.app
export PATH="/Applications/Postgres.app/Contents/Versions/9.3/bin:$PATH"
{% endhighlight %}

All we have to do now is launch `Postgres.app` (found in applications) and hit `Open psql`. In your `bash` terminal, create a new database by running the command

{% highlight bash %}
$ createdb pre-registration
{% endhighlight %}

Now, open up the postgres terminal launched when hitting `Open psql` and run the command

{% highlight bash %}
=# \list
{% endhighlight %}

You should see the database `pre-registration` listed in the table.

`2.)` Setting up our actual Flask app

First, create the following files and folders

{% highlight bash %}
$ mkdir static
$ mkdir templates
$ touch app.py
$ touch .gitignore
{% endhighlight %}

Open up your `.gitignore` file and add the following

{% highlight text %}
venv
*.pyc
.DS_Store
{% endhighlight %}

Now, go into your `static` folder and add following two folders and their respective files

- `css` --> bootstrap.min.css, styles.css (found [here][8])
- `js` --> bootstrap.min.js, scripts.js (found [here][9])

Then, go into your `templates` folder and add the following two HTML files

- `index.html` found [here][10]
- `success.html` found [here][11]

A couple things to notice in the HTML files; links to static files in Flask are done a special way, and calling a method from a form action is done similarly.

Let's install some Flask stuff now. Run the following commands in your terminal

{% highlight bash %}
$ pip install Flask
$ pip install psycopg2
$ pip install Flask-SQLAlchemy
{% endhighlight %}

Next, we have to write the python code to do things and connect our database. Add the following code to your `app.py` file

{% highlight python %}
from flask import Flask, render_template, request
from flask.ext.sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://localhost/pre-registration'
db = SQLAlchemy(app)

# Create our database model
class User(db.Model):
	__tablename__ = "users"
    id = db.Column(db.Integer, primary_key=True)
    email = db.Column(db.String(120), unique=True)

    def __init__(self, email):
        self.email = email

    def __repr__(self):
        return '<E-mail %r>' % self.email

# Set "homepage" to index.html
@app.route('/')
def index():
    return render_template('index.html')

# Save e-mail to database and send to success page
@app.route('/prereg', methods=['POST'])
def prereg():
    email = None
    if request.method == 'POST':
        email = request.form['email']
        # Check that email does not already exist (not a great query, but works)
        if not db.session.query(User).filter(User.email == email).count():
            reg = User(email)
            db.session.add(reg)
            db.session.commit()
            return render_template('success.html')
    return render_template('index.html')

if __name__ == '__main__':
    app.debug = True
    app.run()
{% endhighlight %}

To create our database based off our model, run the following commands

{% highlight bash %}
$ python
>>> from app import db
>>> db.create_all()
>>> exit()
{% endhighlight %}

Now, run `python app.py` in your terminal. You should see `* Running on http://127.0.0.1:5000/`, go to [http://127.0.0.1:5000/][12] and experience beauty.

*Bonus, a great postgres GUI client for Mac OS X is `Induction`. Just go [here](http://inductionapp.com/) to download and install. Launch the app and enter `postgres://localhost/pre-registration` as your database URL. Next, browse your data. :)

`3.)` Deploy to Heroku

Coming `soon`.

[1]: http://flask.pocoo.org/
[2]: http://www.postgresql.org/
[3]: https://www.heroku.com/
[4]: https://github.com/sahildiwan/flask-postgres-heroku
[5]: https://pypi.python.org/pypi/pip
[6]: https://virtualenv.pypa.io/en/latest/virtualenv.html
[7]: http://postgresapp.com/
[8]: https://github.com/sahildiwan/flask-postgres-heroku/tree/master/static/css
[9]: https://github.com/sahildiwan/flask-postgres-heroku/tree/master/static/js
[10]: https://github.com/sahildiwan/flask-postgres-heroku/blob/master/templates/index.html
[11]: https://github.com/sahildiwan/flask-postgres-heroku/blob/master/templates/success.html
[12]: http://127.0.0.1:5000/