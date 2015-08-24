# sequelizejs

sequelize is nodejs' answer to ruby on rail's activerecord, an orm that essentially masks all the
complexities of sql from developers.  i imagine since most users of nodejs prefer nosql
solutions like mongodb, this is a space that lags behind ruby on rails significantly.  but for me,
nosql is in most cases, a pre-mature optimization, most of the time your data has lots of complicated
relations and this can be accomplished well with a relational database.  when you get to maybe
hundreds of thousands of concurrent requests to the persistence layer, then you probably need
to start thinking about using nosql, but alas nodejs came out around the time when nosql was
getting popular so it feels like most of the effort was made for the newer technologies
which is fine.  noted that nodejs is asynchronous in nature, so perhaps orm's are not the most
straightforward to write.  here's my experience with sequelize in the hope that it can help
those that just want to create a traditional application with relational database.

## initial impressions

not certain if it's production ready yet, but the framework seems to be quite robust, is
accommodative for most of the things you need to describe for table relationships.  but
my general feeling is that the documentation and tools are in relatively poor condition,
i couldn't find answers to my most basic questions on stackoverflow or in the sequelize
docs.  i found that things changed in the framework from 2013 to 2014 quite dramatically,
i'm sensing that there's some compatibility breakage in the way that models were defined
or at least a better way to define things that was not apparent at all looking at stackoverflow
comments and documention.  let's take a look at all the components that you'll probably
need:

### sequelize
framework used to map models to tables and handle all the intricate relationships
between your tables, this is close in many aspects to activerecord.

### sequelize-cli
command line tool that generates migrations, creates models, and initializes the
skeleton code, for those from the ruby world, it looks a lot like a set of rake tasks.

### sequelize-fixtures
fixtures for sequelize, this is pretty much a part of activerecord, i believe the sequelize-fixtures
library was added by someone not involved directly with the sequelize framework (domasx2), you
can create json, yaml, and other text based representations of test data to fill your tables
using this.

### umzug
tool used to run tasks like migrations, but could be used outside of sequelize for general task
management.  you'll need this if you want to programmatically run a migration, this has been
moved out of the sequelize framework, though the documentation hasn't been updated thus far
and will speak of a ~migrator.migrate~ call that will just force you to look at source code
to figure things out.

### database libraries
sequelize seems to have pretty good support for most of the mainstream, opensource
databases like mysql, postgresql, sqlite3 amongst some other proprietary databases.

## the nitty gritty

###  install sequelize and dependencies, here's my package.json file:

```
"pg": "~4.4.0",
"sequelize": "~3.4.1",
"sequelize-cli": "~1.7.2",
"sqlite3": "~3.0.9",
"umzug": "~1.6.0"
```

i used sqlite3 for development purposes and then pg for production, you could obviously
put some of these things in the devDependencies.  running npm will put everything to
node_modules in the local directory.

`npm install`

### initialize sequelize project

generally creates directories and a config file to access your database.

`node_modules/.bin/sequelize init`

### create model (and migration file)

i never really used the rails command line to generate migration files and the like, just
because the command line would get too long with all the different attributes of each column,
so i'm not able to give a fair comparison, but it seems pretty helpful.

`node_modules/.bin/sequelize models:create --name [table name] --attributes "comma delimited list of field name:field type"

this generates a file in the `./migrations` folder with the timestamp and model name being added and then a file
in the `./models` folder with the equivalent model js file.  i had to go in and modify all the attributes, like
having allowNull and adding associations, like activerecord, a primary key, createdAt, and updatedAt fields get created
automatically, and you can also override these so that they are not generated automatically.


