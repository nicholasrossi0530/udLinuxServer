# udLinuxServer
ssh -i ubuntu-key.pem grader@34.220.104.43 -p 2200

i. IP: 34.220.104.43 SSH: 2200
ii. http://34.220.104.43/music/1/menu
iii. List of software installed:
    finger, apache2, postgresql, libapache2-mod-wsgi, git, python-pip, flask, sqlalchemy, psycopg2
    Config changes:
    added user grader with sudo permissions, created music_catalog db in postgres with user catalog with all permissions, changed time zone to UTC, added proper ports to firewall and enabled, changed ssh to port 2200, set up grader user with Lightsail key, Amazon Lightsail, added proper ports to Lightsail firewall
iv. Sources used: Udacity classroom forums, Udacity class code, StackOverflow,
    Python forums, Apache2 forums, Lightsail forums
