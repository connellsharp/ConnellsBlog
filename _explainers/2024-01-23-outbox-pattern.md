---
layout: video
title: Transactional Outbox Pattern
tags: architecture events
youtube: tQw99alEVHo
---

The Transactional Outbox Pattern ensures a message is always published to a message broker when making changes to a database, even if the message broker fails initially. This is essential for event-driven architecture to ensure consistency when other services are rebuilding state from your events.