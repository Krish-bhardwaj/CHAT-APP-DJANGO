# Chat App Django  #

This is a P2P messenging app built using django.It has a REST API and uses WebSockets to notify clients of new messages and 
avoid polling.


## Architecture ##
 1. When a user logs in, the frontend downloads the user list and opens a Websocket connection to the server .
 2. When a user selects another user to chat, the frontend downloads the latest 10 messages (see settings) they've exchanged.
 3. When a user sends a message, the frontend sends a POST to the REST API, then Django saves the message and notifies the users involved using the Websocket connection (sends the new message ID).
 4. When the frontend receives a new message notification (with the message ID), it performs a GET query to the API to download the received message.

## Scaling ##
This can be easy scalled up to as many users as we want . We just need to increase the size of backend server (database) inorder to cater large ammount of audience 

### Requests ###
Because Channels takes Django into a multi-process model, you no longer run 
everything in one process along with a WSGI server. Instead you run one or 
more interface servers, and one or more worker servers, connected by that 
channel layer you configured earlier.

In this case, I'm using the **In-Memory channel system**, but could be changed to
the Redis backend to improve performance and spawn multiple workers in a
distributed environment.

### Database ###
For this project Im using a MySQL database setup. If more performance is required, a MySQL cluster / shard could be deployed. I have not yet deployed this project on server . I'm also using indexes to improve performance.

## Assumptions ##
Because of time constraints this project lacks of:

- User Sign-In / Forgot Password
- User Selector Pagination
- Good Test Coverage
- Better Comments / Documentation Strings
- Frontend Tests
- Modern Frontend Framework 
- Frontend Package 
- Proper UX / UI design 

## Run ##

0. move to project root folder


1. Create and activate a virtualenv (Python 3)
```bash
pipenv --python 3 shell
```
2. Install requirements
```bash
pipenv install
```
3. Create a MySQL database
```sql
CREATE DATABASE chat CHARACTER SET utf8;
```
4. Start Redis Server
```bash
redis-server
```

5. Init database
```bash
./manage.py migrate
```
6. Run tests
```bash
./manage.py test
```

7. Create admin user
```bash
./manage.py createsuperuser
```

8. Run development server
```bash
./manage.py runserver
```

To override default settings, create a local_settings.py file in the chat folder.

Message prefetch config (load last 10 messages):
```python
MESSAGES_TO_LOAD = 10
```
