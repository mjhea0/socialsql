# Scenario

**Create a model that maps out a relationship between an artist, album, and producer.**

- Artist - name, stage name, genre
- Producer - name, street address, city, state, website
- Album - title, artists, producer, release date

*An album can have many artists, but only one producer*

# Create the tables (using a SQLite database)

## SQL

    CREATE TABLE "socialsql_producer" (
        "id" integer NOT NULL PRIMARY KEY,
        "name" varchar(30) NOT NULL,
        "address" varchar(50) NOT NULL,
        "city" varchar(60) NOT NULL,
        "state" varchar(3) NOT NULL,
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

    INSERT INTO socialsql_producer VALUES (1, 'Producer 1', '28 Django Drive', 'Lawrence', 'KS', 'http://www.producer1.com/');
    INSERT INTO socialsql_producer VALUES (2, 'Producer 2', '304 Pine Street', 'San Francisco', 'CA', 'http://www.producer2.com/');
    INSERT INTO socialsql_artist VALUES (1, 'Don', 'Donnie Angel', 'Blues');
    INSERT INTO socialsql_artist VALUES (2, 'Frank Zappos', 'Meta', 'Rock');
    INSERT INTO socialsql_artist VALUES(3, 'Bob Dylan', 'Bob Dylan', 'Folk');

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

## SQL
    
    SELECT name, address, city, state, website FROM socialsql_producer;
    SELECT * from socialsql_producer;
    SELECT address, city, state FROM socialsql_producer WHERE name = 'Producer 1';
    SELECT stage_name FROM socialsql_artist;
    SELECT genre FROM socialsql_artist WHERE name = 'Don';

## Django ORM

# Update

## SQL

    UPDATE socialsql_producer SET name = 'Ricky Rangles' WHERE name = 'Producer 1';
    UPDATE socialsql_artist SET genre = 'Rock' WHERE name = 'Bob Dylan';

## Django ORM

# Delete

## SQL

    DELETE FROM socialsql_producer WHERE name = 'Ricky Rangles';
    DELETE FROM socialsql_artist WHERE genre = 'Rock';

## Django ORM

# Fixtures

## SQL

    # Artist table fixture
    B.I.G.:
        name: Christopher George Latore Wallace
        stage_name: Notorious B.I.G.
        genre: Hip hop

    Johnny Cash:
        name: John R. Cash
        stage_name: Johnny Cash
        genre: Country

    Elvis:
        name: Elvis Aaron Presley
        stage_name: Elvis
        genre: Rock and Roll

## Django ORM

# Iterating over results

## SQL

    DECLARE artistCursor CURSOR FOR SELECT name, genre FROM socialsql_artist;
    DECLARE @name VARCHAR(256);
    DECLARE @genre VARCHAR(256);
    OPEN artistCursor;
    FETCH NEXT FROM artistCursor INTO @name, @genre;
    WHILE @@FETCH_STATUS = 0
    BEGIN
        IF @genre = 'Hip Hop'
            DELETE FROM socialsql_artist WHERE name LIKE @name;

        FETCH NEXT FROM artistCursor;
    END

## Django ORM

# Where, order, limit, offset, group

    # Where
    SELECT name FROM socialsql_artist WHERE genre = 'Hip Hop';
    SELECT address FROM socialsql_producer WHERE name = 'Producer 1';

    # ORDER
    SELECT stage_name, genre FROM socialsql_artist ORDER BY genre;
    SELECT * FROM socialsql_producer ORDER BY address DESC;

    # LIMIT
    SELECT * FROM socialsql_artist LIMIT 1, 3;
    SELECT name FROM socialsql_producer LIMIT 0, 3;

    # OFFSET
    SELECT stage_name FROM socialsql_artist LIMIT 1 OFFSET 2;
    SELECT address FROM socialsql_producer LIMIT 2 OFFSET 1;

    # GROUP
    SELECT stage_name FROM socialsql_artist GROUP BY name;

## SQL

## Django ORM

# average, count, max, min, sum

## SQL

## Django ORM

# table/row locking

## SQL

## Django ORM

# 1:1

## SQL

## Django ORM

# 1 to many 1:n (create, read(find associated records))

## SQL

## Django ORM

# join

## SQL

## Django ORM

# many to many n:n (crud)

## SQL

## Django ORM

# seperaye functions for frequent searches

## SQL

## Django ORM

# validation (null, data type, length, max, minm greather than, unique, email)

## SQL

## Django ORM

# default values

## SQL

## Django ORM

# index
 
## SQL

## Django ORM
   
