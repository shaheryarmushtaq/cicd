# cicd
# .gitlab-ci.yml
stages:
  - build
  - deploy
  - test

variables:
  DOCKER_IMAGE: 'sheri'

build:
  stage: build
  tags:
    - sheri
  script:
    - echo "Building Docker image..."
    - docker build -t $DOCKER_IMAGE .

deploy:
  stage: deploy
  tags:
    - sheri
  script:
    - echo "Deploying Docker container..."
    - docker stop sheri || true
    - docker rm sheri || true
    - docker run -d --name sheri -p 8002:8002 -p 8003:8003 $DOCKER_IMAGE

test:
  stage: test
  tags:
    - sheri
  script:
    - echo "Testing if the servers are up..."
    - apt-get update && apt-get install -y curl
    - sleep 10
    - '[[ $(curl -s -o /dev/null -w "%{http_code}" http://103.151.111.237:8002) == "200" ]]'
    - '[[ $(curl -s -o /dev/null -w "%{http_code}" http://103.151.111.237:8003) == "200" ]]'


    # Dockerfile

    FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html
COPY index2.html /usr/share/nginx/html/index2.html

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 8002 8003

CMD ["nginx", "-g", "daemon off;"]


# nginx.conf

worker_processes 1;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

# add in server block
   
    server {
        listen 8002;
        server_name localhost;

        location / {
            
            root /usr/share/nginx/html;
            index index.html;
        }
    }

    server {
        listen 8003;
        server_name localhost;

        location / {
            root /usr/share/nginx/html;
            index index2.html;
        }
    }
}


# index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Muqadas Fiaz - Resume</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');

        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
            line-height: 1.6;
        }
        .container {
            width: 70%;
            margin: 30px auto;
            padding: 20px;
            background-color: #fff;
            border: 1px solid #ddd;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
        }
        h1 {
            font-size: 2.5em;
            color: #34495e;
            border-bottom: 2px solid #3498db;
            padding-bottom: 10px;
            margin-bottom: 20px;
        }
        h2 {
            font-size: 1.8em;
            color: #34495e;
            margin-bottom: 10px;
        }
        h3 {
            font-size: 1.4em;
            color: #2c3e50;
            margin-bottom: 10px;
        }
        .contact-info, .skills, .work-history, .education {
            margin-bottom: 30px;
        }
        .contact-info p, .skills ul, .work-history ul, .education ul {
            margin: 0;
            padding: 0;
            list-style: none;
        }
        .skills ul, .work-history ul, .education ul {
            margin-left: 20px;
        }
        .skills ul li, .work-history ul li, .education ul li {
            margin-bottom: 8px;
        }
        .skills ul li::before, .work-history ul li::before, .education ul li::before {
            content: '•';
            color: #3498db;
            margin-right: 8px;
        }
        .contact-info p {
            font-size: 1.1em;
            margin-bottom: 5px;
        }
        .contact-info p strong {
            color: #2c3e50;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Muqadas Fiaz</h1>
        <div class="contact-info">
            <p><strong>Email:</strong> muqadasmehar07@gmail.com</p>
        </div>

        <div class="skills">
            <h2>Skills</h2>
            <ul>
                <li>Testing and maintenance</li>
                <li>Project budgeting</li>
                <li>Customer relationship management</li>
                <li>Superior time management</li>
                <li>Project Management</li>
                <li>Resource Allocation</li>
                <li>Project planning</li>
                <li>Software development lifecycle expert</li>
            </ul>
        </div>

        <div class="work-history">
            <h2>Work History</h2>
            <h3>Software Engineer, NEPNational Incubation Center Gujrat, Pakistan (2022-04 to 2022-09)</h3>
            <ul>
                <li>Reviewed project specifications and designed technology solutions that met or exceeded performance expectations.</li>
                <li>Worked with software development and testing team members to design and develop robust solutions to meet client requirements for functionality, scalability, and performance.</li>
                <li>Coordinated with other engineers to evaluate and improve software and hardware interfaces.</li>
                <li>Collaborated with management, internal, and development partners regarding software application design status and project progress.</li>
                <li>Analyzed proposed technical solutions based on customer requirements.</li>
            </ul>
        </div>

        <div class="education">
            <h2>Education</h2>
            <ul>
                <li><strong>BS Software Engineering:</strong> Information Technology, University of Gujrat </li>
                <li><strong>Intermediate:</strong> F.s.c Pre-Engineering</li>
                <li><strong>Matriculation:</strong> Biology,science</li>
            </ul>
        </div>
    </div>
</body>
</html>

# index2.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shaheryar Mushtaq - Curriculum Vitae</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
        }
        .section {
            margin-bottom: 20px;
        }
        .section h2 {
            margin-bottom: 10px;
            font-size: 1.2em;
        }
        .section p {
            margin-bottom: 5px;
        }
        .skills-list {
            margin-bottom: 10px;
            padding-left: 20px;
        }
        .skills-list li {
            margin-bottom: 5px;
        }
    </style>
</head>
<body>

<div class="section">
    <h2>Personal Information</h2>
    <p><strong>Name:</strong> Shaheryar Mushtaq</p>
    <p><strong>Address:</strong> Near Arfani Masjid, Moh. Muslimabad, Teh & Dist, Gujrat, 50700, Pakistan</p>
    <p><strong>Phone:</strong> +92 303-6057616</p>
    <p><strong>Email:</strong> <a href="mailto:shaheryar633@gmail.com">shaheryar633@gmail.com</a></p>
</div>

<div class="section">
    <h2>Education</h2>
    <p><strong>University:</strong> University of Gujrat</p>
    <p><strong>Degree:</strong> BS (Computer Science)</p>
    <p><strong>Field of Study:</strong> Computer Science</p>
    <p><strong>Duration:</strong> August 2012 - December 2016</p>
</div>

<div class="section">
    <h2>Skills & Competencies</h2>
    <ul class="skills-list">
        <li>Ability to organize and set priorities.</li>
        <li>Excellent troubleshooting, problem solving and interpersonal skills.</li>
        <li>Ability to identify tasks that require automation and automate them.</li>
    </ul>
    <ul class="skills-list">
        <li>AWS services (EC2, S3, RDS, Lambda, etc.)</li>
        <li>CI/CD pipeline implementation</li>
        <li>Containerization and orchestration (Docker, Kubernetes, etc.)</li>
        <li>Scripting languages (Python, Bash, etc.)</li>
        <li>Version control systems (Git, GitHub, etc.)</li>
        <li>Linux/Unix administration</li>
        <li>Networking and security concepts</li>
        <li>Performance optimization and scaling strategies</li>
        <li>Troubleshooting and problem-solving skills</li>
        <li>Strong communication and teamwork abilities</li>
    </ul>
</div>

</body>
</html>


