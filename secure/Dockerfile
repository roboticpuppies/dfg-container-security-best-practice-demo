# syntax=docker/dockerfile:1

# Correction #1: Multi-stage build
# Compiler, source code and build tools stay in this stage and are NOT shipped.
# Pin the version (never `latest`) so builds are reproducible and predictable.
FROM golang:1.21-alpine AS build
WORKDIR /app

# The whole folder is copied here, but that's fine: only the binary reaches the final image.
# .dockerignore keeps secrets (.env, keys) out of the build context in the first place.
COPY . .
RUN go mod tidy && CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o server .

# Correction #2: Small, pinned base image for the final stage
# Fewer packages = fewer vulnerabilities and less for an attacker to use.
# Alpine balances security and ease of use (Scratch/Distroless are even smaller but have no shell to debug).
FROM alpine:3.21.3 AS final

# Correction #3: Run as a dedicated non-root user
# If the app is hacked, the attacker is not root inside the container.
# A fixed numeric UID lets tools (e.g. Kubernetes runAsNonRoot) verify it.
RUN addgroup -S -g 10001 app && adduser -S -u 10001 -G app app

WORKDIR /app

# Correction #4: Keep the binary owned by root (read + execute only for the app user)
# So the app cannot overwrite its own binary.
COPY --from=build /app/server .

# Correction #5: Switch to the non-root user AFTER all root-only steps are done
USER 10001:10001

# Correction #6: No secrets in the image
# Don't use ENV or ARG for passwords: both are visible with `docker history`.
# Pass them at runtime (docker run -e / secrets), or use `RUN --mount=type=secret` if needed at build time.

# Correction #7: Expose only the port the app needs
EXPOSE 8080

# Exec form: the app runs as PID 1 and receives stop signals properly
CMD ["/app/server"]
