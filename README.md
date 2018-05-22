# cocotask
### Build task queue on either RabbitMQ or Kafka! Simple! Easy! and FAST!!!!

**Why creating this framework?**
* No need to worry about using Rabbitmq or Kafka. Just pick one and go! It's simply a few lines of config changes in future if you want to switch to another
* Hide all details for connecting/subscribing/publishing/etc. on either Kafka or Rabbitmq. You can just use the same API for both!
* Minimize the efforts for team members to handcraft the code for exchange/queue handling
* Most usage for Rabbitmq is simple pub/sub on different exchanges/queues (in my case)
* Team members should focus on how to handle messages
* There lacks good mananger tool/lib to handle creating multiple consumers (although it's simple)
* Celery is actually the best one if it supports windows, unfortunately it's not. So we have to use pure rabbitmq and develop our own tool (somewhat similar to Celery)
<hr>

## Test

1. install rabbitmq or kafka on local machine (either docker or pure rabbitmq). 
   - Rabbitmq: https://www.rabbitmq.com/
   - Kafka: https://kafka.apache.org/quickstart  (for kafka, you have to manually create a topic named `test_topic_1` in order to run the test. In order to try multiple consumers, you need to set partitions to 2 or above, not 1)

2. make sure it's running Python 3.5 above

3. pip install pika

4. pip install cocotask

5. now you have cocotask setup on your machine. To test, you can go to the ./test folder
   - Run: `python producer_test.py`   (this will post a string. Code is very simple)
   - In another window, under ./test, run: `cocotask ./config.json userworkers TestWorker 4`, and you'll see the worker starts and process 1 message we just posted.

6. So the key part is how we use cocotask command tool. the parameters are:
   - config_path: path to config file
   - module_name: module of your customized worker class
   - class_name: the class name of your own worker class (in the example above, it's userworkers.TestWorker, so the module name is userworkers, the class name is TestWorker)
   - number of workers: the total number of customer workers
   - logginglevel(optional): python logging level INFO/DEBUG/etc.
   - modulepath(optional): the relative path to find the module you defined. Default is '.'

7. Develop your own worker class and try

That's it. Simple and straightforward.
