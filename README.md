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

`.github/workflows/docker-build.yml` uses GitHub's `ubuntu-latest` hosted runner to build and publish the root `Dockerfile` to Docker Hub and GitHub Container Registry on every branch push. It can also be started manually from the Actions tab. Images are tagged with the commit SHA, and the default branch also updates `latest`. The workflow summary displays the published tags, pull command, and image digest.

For Docker Hub publishing, add these repository Actions secrets under **Settings → Secrets and variables → Actions**:

- `DOCKERHUB_USERNAME`: Docker Hub username (`devopseasylearning`)
- `DOCKERHUB_TOKEN`: Docker Hub access token with read/write permissions

The workflow uses the automatically provided `GITHUB_TOKEN` for GitHub Container Registry. Do not commit registry credentials to the repository.

Pull the latest image from Docker Hub with:

```bash
docker pull devopseasylearning/s13-training-camps:latest
```

Or from GitHub Container Registry:

```bash
docker pull ghcr.io/s13ulissia/s13-docker:latest
```

GitHub packages may be private by default. If so, sign in to `ghcr.io` with a GitHub token that has `read:packages` permission, or change the package visibility in the repository's package settings.
