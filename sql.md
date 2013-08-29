# Scenario

**Create a model that maps out a relationship between an artist, album, and producer.**

- Artist - name, stage name, genre
- Producer - name, street address, city, state, website
- Album - title, artists, producer, release date
- An album can have many artists, but only one producer

# Create the tables (using a SQLite database)

## SQL

    CREATE TABLE "socialsql_producer" (
        "id" integer NOT NULL PRIMARY KEY,
        "name" varchar(30) NOT NULL,
        "address" varchar(50) NOT NULL,
        "city" varchar(60) NOT NULL,
        "state" varchar(30) NOT NULL,
        "website" varchar(200) NOT NULL
    )
    ;
    CREATE TABLE "socialsql_artist" (
        "id" integer NOT NULL PRIMARY KEY,
        "name" varchar(30) NOT NULL,
        "stage_name" varchar(30) NOT NULL,
        "genre" varchar(30) NOT NULL
    )
    ;
    CREATE TABLE "socialsql_album_artists" (
        "id" integer NOT NULL PRIMARY KEY,
        "album_id" integer NOT NULL,
        "artist_id" integer NOT NULL REFERENCES "socialsql_artist" ("id"),
        UNIQUE ("album_id", "artist_id")
    )
    ;
    CREATE TABLE "socialsql_album" (
        "id" integer NOT NULL PRIMARY KEY,
        "title" varchar(100) NOT NULL,
        "producer_id" integer NOT NULL REFERENCES "socialsql_producer" ("id"),
        "release_date" date NOT NULL
    )
    ;

    COMMIT;

## Django ORM

    class Producer(models.Model):
    	name = models.CharField(max_length=30)
    	address = models.CharField(max_length=50)
    	city = models.CharField(max_length=60)
    	state = models.CharField(max_length=2)
    	website = models.URLField()
    
    class Artist(models.Model):
    	name = models.CharField(max_length=30)
    	stage_name = models.CharField(max_length=30)
    	genre = models.CharField(max_length=30)
    
    class Album(models.Model):
    	title = models.CharField(max_length=100)
    	artists = models.ManyToManyField(Artist)
    	producer = models.ForeignKey(Producer)
    	release_date = models.DateField()
    	
# Create

## SQL

## Django ORM

    $ python manage.py shell
    >>> from socialsql.models import *
    >>> p1 = Producer(name='Producer 1', address='28 Django Drive', city='Lawrence', state='KS', website='http://www.producer1.com/')
    >>> p1.save()
    >>> p2 = Producer(name='Producer 2', address='304 Pine Street', city='San Francisco', state='CA', website='http://www.producer2.com/')
    >>> p2.save()
    >>> a1 = Artist(name='Don',stage_name='Donnie Angel',genre='Blues')
    >>> a1.save()
    >>> a2 = Artist(name='Frank Zappos',stage_name='Meta',genre='Rock')
    >>> a2.save()
    >>> a1 = Artist(name='',stage_name='',genre='')
    
# Read

# Update

# Delete

# Fixtures

# Iterating over results

# Where, order, limit, offset, group

# average, count, max, min, sum

# table/row locking

# 1:1

# 1 to many 1:n (create, read(find associated records))

# join

# many to many n:n (crud)

# seperaye functions for frequent searches

# validation (null, data type, length, max, minm greather than, unique, email)

# default values

# index
    
