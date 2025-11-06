---
title: "wordpress docker setup"
date: 2025-05-05
slug: "wordpress-docker-setup"
draft: false
---

The author shares a Docker configuration for quickly deploying WordPress, developed while creating a WooCommerce extension. The setup involves two main components:

## Docker Compose Configuration

The article provides a `docker-compose.yml` file featuring a WordPress service and MySQL database. The WordPress container exposes port 12999 and connects to the database using environment variables. The setup includes volume mounting for `wp-content` directory and establishes networking between services using a bridge network called `wp-network`. The MySQL database is configured with persistent storage through named volumes.

## Nginx Reverse Proxy Setup

A critical configuration element involves setting up an Nginx reverse proxy with HTTPS support. The author emphasizes that "you need to set https headers for the proxy otherwise wordpress will serve the content back in mixed mode" and cause browser complaints.

Key proxy headers include:
- `X-Forwarded-Proto https` for protocol preservation
- HTTP/1.1 upgrade settings for connection compatibility
- Host header configuration for proper routing

The configuration integrates with Let's Encrypt SSL certificates for HTTPS termination, creating a secure connection between the reverse proxy and clients while communicating with the containerized WordPress installation.
