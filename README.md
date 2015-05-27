# Amazon start and stop

This is a sample "script" to schedule start and stop [Amazon EC2 instances](https://aws.amazon.com/) based on a date time range, i.e. start at 6AM and stop at 12AM.

Have a look at [AWS Calculator](https://calculator.s3.amazonaws.com/calc5.html) to see how much you can save by stopping unsed instances from 12AM to 6AM (save 25%) or other date time ranges.


## Usage

### Checkout amazon_start_stop.

```bash
$ git clone git@github.com:austra/amazon_start_stop.git
$ cd amazon_start_stop
```

### Heroku

1. Create a [Heroku account](https://www.heroku.com/) if don't have one and install the [Heroku Toolbelt](https://toolbelt.heroku.com/).
2. Create a [new app](https://devcenter.heroku.com/articles/creating-apps).

```bash
$ heroku apps:create
```

### Deploy

```bash
$ git push heroku master
```

### Configure Environment Variables

Configure your app settings using [Heroku config vars](https://devcenter.heroku.com/articles/config-vars).

Get your AWS credentials here:
[AWS Credentials](http://aws.amazon.com/iam/) 

Rename `.env.sample` to `.env` or Create a file named `.env` and add the following, customized to your needs:

```bash
# .env
AWS_ACCESS_KEY_ID=""
AWS_SECRET_ACCESS_KEY=""

# Leave blank if you don't need to associate an Elastic IP
# (everytime you stop an instance Amazon dissociates Elastic IPs)
AWS_ELASTIC_IP=""

# The instance id to start and stop i.e. i-1a1aa111
AWS_INSTANCE_ID=""
AWS_REGION="us-west-2"

LANG="en_US.UTF-8"
START_TIME="9:45pm"
STOP_TIME="10:45pm"
TIMEZONE="America/Denver"

Run the following to create the Herkou Environment Variables from your `.env` file.

$ heroku plugins:install git://github.com/ddollar/heroku-config.git
$ heroku config:push 
```

### Heroku Scheduler

Add and configure [Heroku Scheduler add-on standard](https://devcenter.heroku.com/articles/scheduler) (free add-on).

```bash
$ heroku addons:add scheduler:standard
$ heroku addons:open scheduler
```

In the Scheduler Dashboard add a new Scheduler.

| TASK                    | DYNO SIZE  | FREQUENCY        |
| ----------------------- |:----------:| ----------------:|
| bin/amazon_start_stop   | 1x         | Every 10 minutes |

### Manual execution

In production.

```bash
$ heroku run bundle exec ruby bin/amazon_start_stop
# to check the script output
$ heroku logs
```

In development.

```bash
$ foreman start
```

