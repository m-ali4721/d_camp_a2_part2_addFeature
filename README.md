After adding feature:

docker compose up --build -d

=> The command has been used to start the containers to make the application accessible
-----------------------------------------------------------------------------
[+] Building 2.2s (11/11) FINISHED                                                       docker:default
 => [flask_app internal] load build definition from Dockerfile                                     0.0s
 => => transferring dockerfile: 220B                                                               0.0s
 => [flask_app internal] load metadata for docker.io/library/python:3.6-slim-buster                2.1s
 => [flask_app auth] library/python:pull token for registry-1.docker.io                            0.0s
 => [flask_app internal] load .dockerignore                                                        0.0s
 => => transferring context: 2B                                                                    0.0s
 => [flask_app 1/5] FROM docker.io/library/python:3.6-slim-buster@sha256:e10aa83604948c6d8d9f72a9  0.0s
 => [flask_app internal] load build context                                                        0.0s
 => => transferring context: 131B                                                                  0.0s
 => CACHED [flask_app 2/5] WORKDIR /app                                                            0.0s
 => CACHED [flask_app 3/5] COPY requirements.txt ./                                                0.0s
 => CACHED [flask_app 4/5] RUN pip install -r requirements.txt                                     0.0s
 => CACHED [flask_app 5/5] COPY . .                                                                0.0s
 => [flask_app] exporting to image                                                                 0.0s
 => => exporting layers                                                                            0.0s
 => => writing image sha256:7f80573675ce3b835f51a1978c0bf4b37a59a22a3664b9e03a49fca8e7917ffe       0.0s
 => => naming to docker.io/m-ali/flask_app:1.0.0                                                   0.0s
[+] Running 2/2
 ✔ Container flask_db   Started                                                                    0.0s 
 ✔ Container flask_app  Started         

--------------------------------------------------------------------------------
docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED       STATUS              PORTS                                       NAMES
d44a3e1aeba7   m-ali/flask_app:1.0.0   "flask run --host=0.…"   3 hours ago   Up About a minute   0.0.0.0:4000->4000/tcp, :::4000->4000/tcp   flask_app
03a3290a3f1b   postgres:12             "docker-entrypoint.s…"   3 hours ago   Up About a minute   0.0.0.0:5434->5432/tcp, :::5434->5432/tcp   flask_db

-----------------------------------------------------------------------------------Scaling:

docker compose up --build --scale flask_app=2 -d

=> The command has been used to scale the web side of the application to two containers as a scaling policy

docker ps

CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS                                         NAMES
dbc3d1f2fc6b   m-ali/flask_app:1.0.0   "flask run --host=0.…"   8 seconds ago   Up 7 seconds   0.0.0.0:32771->4000/tcp, :::32771->4000/tcp   a2_part2_addfeature-flask_app-1
071ffe45304d   m-ali/flask_app:1.0.0   "flask run --host=0.…"   8 seconds ago   Up 6 seconds   0.0.0.0:32772->4000/tcp, :::32772->4000/tcp   a2_part2_addfeature-flask_app-2
146c16726fe1   postgres:12             "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   0.0.0.0:5434->5432/tcp, :::5434->5432/tcp     flask_db

Output:

docker compose up --build --scale flask_app=2 -d
[+] Building 1.2s (10/10) FINISHED                                                       docker:default
 => [flask_app internal] load build definition from Dockerfile                                     0.0s
 => => transferring dockerfile: 220B                                                               0.0s
 => [flask_app internal] load metadata for docker.io/library/python:3.6-slim-buster                1.1s
 => [flask_app internal] load .dockerignore                                                        0.0s
 => => transferring context: 2B                                                                    0.0s
 => [flask_app internal] load build context                                                        0.0s
 => => transferring context: 157B                                                                  0.0s
 => [flask_app 1/5] FROM docker.io/library/python:3.6-slim-buster@sha256:e10aa83604948c6d8d9f72a9  0.0s
 => CACHED [flask_app 2/5] WORKDIR /app                                                            0.0s
 => CACHED [flask_app 3/5] COPY requirements.txt ./                                                0.0s
 => CACHED [flask_app 4/5] RUN pip install -r requirements.txt                                     0.0s
 => CACHED [flask_app 5/5] COPY . .                                                                0.0s
 => [flask_app] exporting to image                                                                 0.0s
 => => exporting layers                                                                            0.0s
 => => writing image sha256:35302c9b0814a82833bcf461bd5772452f3fa09a6e37eb009cd0f0dd246bc4aa       0.0s
 => => naming to docker.io/m-ali/flask_app:1.0.0                                                   0.0s
[+] Running 4/4
 ✔ Network a2_part2_addfeature_default        Created                                              0.1s 
 ✔ Container flask_db                         Started                                              0.0s 
 ✔ Container a2_part2_addfeature-flask_app-2  Start...                                             0.1s 
 ✔ Container a2_part2_addfeature-flask_app-1  Start...                                             0.1s 

 
docker logs dbc3d1f2fc6b

 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
/usr/local/lib/python3.6/site-packages/flask_sqlalchemy/__init__.py:873: FSADeprecationWarning: SQLALCHEMY_TRACK_MODIFICATIONS adds significant overhead and will be disabled by default in the future.  Set it to True or False to suppress this warning.
  'SQLALCHEMY_TRACK_MODIFICATIONS adds significant overhead and '
 * Running on all addresses.
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://172.24.0.3:4000/ (Press CTRL+C to quit)


-------------------------------------------------------------------------------------------------------------------------------------------------------