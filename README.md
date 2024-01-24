After adding feature:

docker compose up --build -d

=> The command has been used to start the containers to make the application accessible

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

-----------------------------------------------------------------------------------
Scaling:

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

Using Prometheus to monitor Web Application:

docker ps


CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS                                       NAMES
c7c9f0ae4a3a   prom/prometheus         "/bin/prometheus --c…"   3 minutes ago    Up 3 minutes    0.0.0.0:9090->9090/tcp, :::9090->9090/tcp   a2_part2_addfeature-prometheus-1
e6e3c68d8bc3   m-ali/flask_app:1.0.0   "flask run --host=0.…"   3 minutes ago    Up 3 minutes    0.0.0.0:4000->4000/tcp, :::4000->4000/tcp   a2_part2_addfeature-flask_app-1
0634df87656c   postgres:12             "docker-entrypoint.s…"   24 minutes ago   Up 24 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   flask_db

Logs for Prometheus container:

docker logs c7c9f0ae4a3a
ts=2024-01-23T15:06:03.391Z caller=main.go:544 level=info msg="No time or size retention was set so using the default time retention" duration=15d
ts=2024-01-23T15:06:03.391Z caller=main.go:588 level=info msg="Starting Prometheus Server" mode=server version="(version=2.49.1, branch=HEAD, revision=43e14844a33b65e2a396e3944272af8b3a494071)"
ts=2024-01-23T15:06:03.391Z caller=main.go:593 level=info build_context="(go=go1.21.6, platform=linux/amd64, user=root@6d5f4c649d25, date=20240115-16:58:43, tags=netgo,builtinassets,stringlabels)"
ts=2024-01-23T15:06:03.391Z caller=main.go:594 level=info host_details="(Linux 6.2.0-39-generic #40~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Thu Nov 16 10:53:04 UTC 2 x86_64 c7c9f0ae4a3a (none))"
ts=2024-01-23T15:06:03.391Z caller=main.go:595 level=info fd_limits="(soft=1048576, hard=1048576)"
ts=2024-01-23T15:06:03.391Z caller=main.go:596 level=info vm_limits="(soft=unlimited, hard=unlimited)"
ts=2024-01-23T15:06:03.394Z caller=web.go:565 level=info component=web msg="Start listening for connections" address=0.0.0.0:9090
ts=2024-01-23T15:06:03.395Z caller=main.go:1039 level=info msg="Starting TSDB ..."
ts=2024-01-23T15:06:03.396Z caller=tls_config.go:274 level=info component=web msg="Listening on" address=[::]:9090
ts=2024-01-23T15:06:03.396Z caller=tls_config.go:277 level=info component=web msg="TLS is disabled." http2=false address=[::]:9090
ts=2024-01-23T15:06:03.401Z caller=head.go:606 level=info component=tsdb msg="Replaying on-disk memory mappable chunks if any"
ts=2024-01-23T15:06:03.401Z caller=head.go:687 level=info component=tsdb msg="On-disk memory mappable chunks replay completed" duration=35.794µs
ts=2024-01-23T15:06:03.401Z caller=head.go:695 level=info component=tsdb msg="Replaying WAL, this may take a while"
ts=2024-01-23T15:06:03.402Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=0 maxSegment=8
ts=2024-01-23T15:06:03.403Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=1 maxSegment=8
ts=2024-01-23T15:06:03.404Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=2 maxSegment=8
ts=2024-01-23T15:06:03.404Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=3 maxSegment=8
ts=2024-01-23T15:06:03.404Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=4 maxSegment=8
ts=2024-01-23T15:06:03.405Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=5 maxSegment=8
ts=2024-01-23T15:06:03.405Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=6 maxSegment=8
ts=2024-01-23T15:06:03.406Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=7 maxSegment=8
ts=2024-01-23T15:06:03.407Z caller=head.go:766 level=info component=tsdb msg="WAL segment loaded" segment=8 maxSegment=8
ts=2024-01-23T15:06:03.407Z caller=head.go:803 level=info component=tsdb msg="WAL replay completed" checkpoint_replay_duration=65.005µs wal_replay_duration=5.357077ms wbl_replay_duration=182ns total_replay_duration=5.481748ms
ts=2024-01-23T15:06:03.410Z caller=main.go:1060 level=info fs_type=EXT4_SUPER_MAGIC
ts=2024-01-23T15:06:03.410Z caller=main.go:1063 level=info msg="TSDB started"
ts=2024-01-23T15:06:03.410Z caller=main.go:1245 level=info msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
ts=2024-01-23T15:06:03.411Z caller=main.go:1282 level=info msg="Completed loading of configuration file" filename=/etc/prometheus/prometheus.yml totalDuration=628.142µs db_storage=1.245µs remote_storage=1.308µs web_handler=417ns query_engine=1.089µs scrape=337.316µs scrape_sd=36.697µs notify=669ns notify_sd=1.376µs rules=1.182µs tracing=5.303µs
ts=2024-01-23T15:06:03.411Z caller=main.go:1024 level=info msg="Server is ready to receive web requests."
ts=2024-01-23T15:06:03.411Z caller=manager.go:146 level=info component="rule manager" msg="Starting rule manager..."


Prometheus accessible at localhost:9090 and showing Web app with status Up and metrics are available as well:

flask-app1 (1/1 up)
Endpoint	State	Labels	Last Scrape	Scrape Duration	Error
http://flask_app:4000/metrics	UP	
instance="flask_app:4000"job="flask-app1"
51.665s ago	
7.373ms

--------------------------------------------------------------------------------------------------------------------------------------------------------------

Prometheus integrated with Graffana:

Query for Dashboard:

{__name__="flask_http_request_created", instance="flask_app:4000", job="flask-app1", method="GET", status="200"}


Output/Logs:

referer="http://localhost:3000/dashboard/new?editPanel=1&orgId=1" handler=/dashboard/new
logger=authn.service t=2024-01-24T06:23:09.07085599Z level=warn msg="Failed to authenticate request" client=auth.client.session error="user token not found"
logger=authn.service t=2024-01-24T06:23:32.504494914Z level=warn msg="Failed to authenticate request" client=auth.client.session error="user token not found"
logger=context userId=1 orgId=1 uname=admin t=2024-01-24T06:23:38.300417598Z level=info msg="Request Completed" method=GET path=/api/live/ws status=-1 remote_addr=172.25.0.1 time_ms=1 duration=1.716262ms size=0 referer= handler=/api/live/ws
logger=infra.usagestats t=2024-01-24T06:23:41.42929222Z level=info msg="Usage stats are ready to report"
logger=context userId=1 orgId=1 uname=admin t=2024-01-24T06:24:58.492137258Z level=info msg="Request Completed" method=GET path=/api/plugins/grafana-llm-app/settings status=404 remote_addr=172.25.0.1 time_ms=1 duration=1.28469ms size=64 referer="http://localhost:3000/dashboard/new?editPanel=1&orgId=1" handler=/api/plugins/:pluginId/settings
logger=context userId=1 orgId=1 uname=admin t=2024-01-24T06:24:58.494693202Z level=info msg="Request Completed" method=GET path=/api/plugins/grafana-llm-app/settings status=404 remote_addr=172.25.0.1 time_ms=1 duration=1.075999ms size=64 referer="http://localhost:3000/dashboard/new?editPanel=1&orgId=1" handler=/api/plugins/:pluginId/settings


docker ps

CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS         PORTS                                       NAMES
7e94973e3303   grafana/grafana         "/run.sh"                10 minutes ago   Up 8 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   a2_part2_addfeature-grafana-1
a9a3a042c4cc   prom/prometheus         "/bin/prometheus --c…"   10 minutes ago   Up 8 minutes   0.0.0.0:9090->9090/tcp, :::9090->9090/tcp   a2_part2_addfeature-prometheus-1
7aaf95248eee   m-ali/flask_app:1.0.0   "flask run --host=0.…"   10 minutes ago   Up 8 minutes   0.0.0.0:4000->4000/tcp, :::4000->4000/tcp   a2_part2_addfeature-flask_app-1
0634df87656c   postgres:12             "docker-entrypoint.s…"   16 hours ago     Up 8 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   flask_db

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------