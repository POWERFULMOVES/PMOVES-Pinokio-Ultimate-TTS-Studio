# PMOVES.AI Integration Guide for Pinokio Ultimate TTS

## Integration Complete

The PMOVES.AI integration template has been applied to Pinokio Ultimate TTS.

## Next Steps

### 1. Customize Environment Variables

Edit the following files with your service-specific values:

- `env.shared` - Base environment configuration
- `env.tier-media` - MEDIA tier specific configuration
- `chit/secrets_manifest_v2.yaml` - Add your service's required secrets

### 2. Update Docker Compose

Add the PMOVES.AI environment anchor to your `docker-compose.yml`:

```yaml
services:
  pinokio-ultimate-tts:
    <<: [*env-tier-media, *pmoves-healthcheck]
    # Your existing service configuration...
```

### 3. Integrate Health Check

Add the health check endpoint to your service:

```python
from pmoves_health import add_custom_check, get_health_status

@app.get("/healthz")
async def health_check():
    return await get_health_status()
```

### 4. Add Service Announcement

Add NATS service announcement to your startup:

```python
from pmoves_announcer import announce_service

@app.on_event("startup")
async def startup():
    await announce_service(
        slug="pinokio-ultimate-tts",
        name="Pinokio Ultimate TTS",
        url=f"http://pinokio-ultimate-tts:7862",
        port=7862,
        tier="media"
    )
```

### 5. Test Integration

```bash
# Test health check
curl http://localhost:7862/healthz

# Verify environment variables loaded
docker compose exec pinokio-ultimate-tts env | grep PMOVES

# Verify NATS announcement
nats sub "services.announce.v1"
```

## Service Details

- **Name:** Pinokio Ultimate TTS
- **Slug:** pinokio-ultimate-tts
- **Tier:** media
- **Port:** 7862
- **Health Check:** http://localhost:7862/healthz
- **NATS Enabled:** True
- **GPU Enabled:** False

## Files Created

- `env.shared` - Base PMOVES.AI environment
- `env.tier-media` - Tier-specific environment
- `chit/secrets_manifest_v2.yaml` - CHIT secrets configuration
- `pmoves_health/` - Health check module
- `pmoves_announcer/` - NATS service announcer
- `pmoves_registry/` - Service registry client
- `docker-compose.pmoves.yml` - PMOVES.AI YAML anchors

## Support

For questions or issues, see the PMOVES.AI documentation.
