5000# Classroom Management System

## Description
 Developed a facial recognition-based automated attendance system for accurate student tracking, integrated with secure login features for both teachers and students. Implemented seamless communication channels between students and faculty, along with the ability to store assignments, documents, and exam results.

## 📢 Explore More on LinkedIn!
Check out our detailed project insights, media, and updates on LinkedIn <b>project section</b> . Click the link below to learn more about IntelliTeach and see it in action!
https://www.linkedin.com/in/hanssaini2005/
## Features
- User-friendly interface for easy navigation and usage
- Secure authentication and access control for different user roles
- Class scheduling and timetable management
- Attendance tracking and reporting
- Gradebook management and progress tracking
- Communication tools for teachers and students (announcements, notifications)
- Reporting and analytics for administrators

## Usage
1. Access the application through your web browser
2. Sign in with your credentials (Hod, Faculity, or student)
3. Explore the different features and functionalities available
4. Perform administrative tasks (if applicable)
5. Manage classes, attendance, assignments, and grades
6. Communicate with teachers and students
7. Generate reports and analyze data


# Requirements

To set up the project, you will need to install the dependencies listed in the `req.txt` file. You can do this by running the following command:

```bash 
pip3 install -r req.txt

sudo apt install rabbitmq-server
```

To collect static files, you will need to run the following command:

```bash
python3 manage.py collectstatic
```

<!-- To generate Tailwind CSS, run the following command: -->

<!-- ```bash
 npx tailwindcss -i input.css -o ./Home/static/public/css/base.css --watch
``` -->


# Running the Application

To run the application, you will need to start the Django development server by running the following command:

```bash
python3 manage.py runserver
```

You will also need to start the Celery worker and beat processes by running the following commands:

```bash
celery -A IgCMS worker

celery -A IgCMS beat
```

To run the application in a production environment, you can use Gunicorn to serve the application. You can do this by running the following command:

```bash
gunicorn IgCMS.wsgi:application --bind 0.0.0.0:8000 --timeout 300
```


# Setup Nginx

install Nginx

```bash
sudo apt update 
sudo apt install nginx
```

You can change nginx server configuration file by running the following command:

```bash
sudo nano /etc/nginx/nginx.conf
```

Add the following line to the `http` block:

```nginx
client_max_body_size 500M;
proxy_read_timeout 300s;
```

To enable the Nginx service to start on boot, you can run the following command:

```bash
sudo systemctl enable nginx
```




To set up Nginx as a reverse proxy server for the application, you will need to create a new server block configuration file in the `/etc/nginx/sites-available/` directory. You can use the following configuration as a template:

```nginx
server {
    listen 80;
    server_name 0.0.0.0;

    # Location for static files
    location /static/ {
        alias /home/hans/Documents/IgCMS/static/;
        autoindex on;
        index index.html;
    }

    # Location for media files
    location /media/ {
        alias /home/hans/Documents/IgCMS/media/;
        autoindex on;
        index index.html;
    }

    # Proxy to Gunicorn
    location / {
        proxy_pass http://0.0.0.0:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Don't forget to replace the paths with the correct paths for your project.

After creating the configuration file, you will need to create a symbolic link to the `/etc/nginx/sites-enabled/` directory. You can do this by running the following command:

```bash
sudo ln -s /etc/nginx/sites-available/igcms /etc/nginx/sites-enabled/
```

Finally, you can restart the Nginx service to apply the changes:

```bash
sudo systemctl restart nginx
```

# Important Commands

for find the process running on port 8000

```bash
sudo lsof -i :8000
```

for kill the process running on port 8000

```bash
kill -9 <PID>
```

