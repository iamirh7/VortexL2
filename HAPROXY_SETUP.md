# HAProxy Installation & Setup for VortexL2

VortexL2 now uses **HAProxy** for high-performance port forwarding instead of pure Python asyncio.

## Installation

### Prerequisites

1. **Install HAProxy:**
   ```bash
   sudo apt-get update
   sudo apt-get install haproxy
   ```

2. **Verify Installation:**
   ```bash
   haproxy -v
   ```

## Configuration

HAProxy configuration is automatically managed by VortexL2 when you:
- Add port forwards
- Remove port forwards  
- Start/stop the forward daemon

The configuration is created at: `/etc/vortexl2/haproxy/vortexl2.cfg`

## Performance Improvements

HAProxy provides **significant performance improvements** over pure Python forwarding:

- **Lower Latency:** C-based implementation with minimal overhead
- **Higher Throughput:** Handles 10,000+ concurrent connections
- **Better Resource Usage:** Lower CPU and memory consumption
- **Production Ready:** Used by major organizations (AWS, Netflix, etc.)

## Statistics

View HAProxy statistics via the management socket:

```bash
echo "show stat" | socat stdio /var/run/haproxy.sock
```

## Troubleshooting

### HAProxy fails to start

1. Check configuration validity:
   ```bash
   sudo haproxy -c -f /etc/vortexl2/haproxy/vortexl2.cfg
   ```

2. Check logs:
   ```bash
   sudo journalctl -u haproxy -f
   ```

3. Ensure ports aren't already in use:
   ```bash
   sudo ss -tlnp | grep haproxy
   ```

### Port forward not working

1. Verify HAProxy is running:
   ```bash
   sudo systemctl status haproxy
   ```

2. Check configuration was generated:
   ```bash
   cat /etc/vortexl2/haproxy/vortexl2.cfg
   ```

3. Verify remote host is reachable:
   ```bash
   ping <remote-forward-ip>
   ```
