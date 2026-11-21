# S13 Training Camps

A self-contained student training website that runs as one Docker container.

## Single-container architecture

The `app` container provides everything:

- Responsive HTML, CSS, and JavaScript website
- Course catalog API
- Student registration and login API
- Enrollment API
- File-based demo persistence through the `app-data` Docker volume

The application uses only the Node.js standard library and has no package dependencies.

## Tools used

- HTML5, CSS3, and vanilla JavaScript
- Node.js 22
- Docker and Docker Compose
- GitHub Actions
- Git and curl

## Run

```bash
docker build -t s13-training-camps:latest .
docker volume create s13-training-camps-data
docker run -d --name s13-training-camps --restart unless-stopped -p 8080:8080 -v s13-training-camps-data:/data s13-training-camps:latest
```

Open <http://localhost:8080>.

## Verify

```bash
docker ps --filter name=s13-training-camps
curl http://localhost:8080/health
curl http://localhost:8080/api/courses
```

## Stop

```bash
docker rm -f s13-training-camps
```

Use `docker volume rm s13-training-camps-data` only when you intentionally want to delete saved demo users and enrollments.

## Continuous integration

`.github/workflows/docker-build.yml` uses GitHub's `ubuntu-latest` hosted runner to build the root `Dockerfile` on every branch push. It can also be started manually from the Actions tab. This runs for commits after they are pushed; it builds the image in the runner but does not publish it to a container registry. No repository secrets are required.
