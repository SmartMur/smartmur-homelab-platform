# Quick Reference: Proxmox HA ARR Stack

The IP addresses below are documentation examples. Substitute your deployment's addresses before using these commands.

## Current Setup
- **Cluster**: Agape (2 nodes: alpha, pve)
- **QDevice**: Pi4 (murzpi) at 198.51.100.83
- **Storage**: PBS, local-lvm, unifiDrive (CIFS)
- **Status**: Ready for HA deployment

## Quick Commands

### Cluster Status
```bash
ssh root@203.0.113.100 pvecm status
```

### QDevice Status
```bash
ssh root@203.0.113.100 corosync-qdevice-net status
```

### HA Status
```bash
ssh root@203.0.113.100 ha-manager status
```

### List VMs
```bash
ssh root@203.0.113.100 qm list
```

### VM Console
```bash
ssh root@203.0.113.100 qm terminal 100
```

### Docker Compose on VM
```bash
ssh root@203.0.113.100 qm exec 100 docker compose -f /opt/homemedia/docker-compose.yml ps
```

## Deployment Steps

### 1. Setup QDevice (if not done)
```bash
cd /Users/dre/Desktop/Smartmur/Terraform
chmod +x setup-qdevice-ha.sh
scp setup-qdevice-ha.sh root@203.0.113.100:/tmp/
ssh root@203.0.113.100 /tmp/setup-qdevice-ha.sh
```

### 2. Configure Terraform
```bash
cd /Users/dre/Desktop/Smartmur/Terraform
cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars with your values
```

### 3. Deploy VMs
```bash
terraform init
terraform plan
terraform apply
```

### 4. Verify Deployment
```bash
# Check cluster
ssh root@203.0.113.100 pvecm status

# Check VMs
ssh root@203.0.113.100 qm list

# Check HA
ssh root@203.0.113.100 ha-manager status

# Check ARR stack
ssh root@203.0.113.100 qm exec 100 docker compose -f /opt/homemedia/docker-compose.yml ps
```

## Monitoring

### Watch Cluster
```bash
watch -n 2 'ssh root@203.0.113.100 pvecm status'
```

### Watch QDevice
```bash
watch -n 2 'ssh root@203.0.113.100 corosync-qdevice-net status'
```

### Watch Logs
```bash
ssh root@203.0.113.100 journalctl -u corosync -f
```

## Troubleshooting

### QDevice Not Connected
```bash
# Check Pi4
ssh root@198.51.100.83 systemctl status corosync-qnetd

# Check connectivity
ssh root@203.0.113.100 nc -zv 198.51.100.83 5403

# Restart
ssh root@203.0.113.100 systemctl restart corosync-qdevice
```

### Quorum Lost
```bash
# Check status
ssh root@203.0.113.100 pvecm status

# Check logs
ssh root@203.0.113.100 journalctl -u corosync -n 50

# Check network
ssh root@203.0.113.100 ping -c 3 203.0.113.200
ssh root@203.0.113.100 ping -c 3 198.51.100.83
```

### VM Not Starting
```bash
# Check HA status
ssh root@203.0.113.100 ha-manager status

# Check VM logs
ssh root@203.0.113.100 qm status 100

# Check cloud-init
ssh root@203.0.113.100 qm exec 100 cloud-init status
```

## Important Files

- `setup-qdevice-ha.sh` - QDevice setup
- `HA-SETUP-GUIDE.md` - Detailed HA guide
- `README-HA-ARR-STACK.md` - Full documentation
- `terraform.tfvars` - Your configuration
- `main.tf` - VM definitions
- `templates/user-data.yaml` - Cloud-init script
- `templates/network-config.yaml` - Network config

## SSH Access

```bash
# Proxmox nodes
ssh root@203.0.113.100
ssh root@203.0.113.200

# Pi4
ssh root@198.51.100.83

# VMs (after deployment)
ssh root@<vm-ip>
```

## Proxmox Web UI

- Node 1: https://203.0.113.100:8006
- Node 2: https://203.0.113.200:8006

## Next Steps

1. ✅ SSH keys configured
2. ⏳ QDevice HA setup (run setup-qdevice-ha.sh)
3. ⏳ Configure terraform.tfvars
4. ⏳ Deploy VMs with Terraform
5. ⏳ Verify ARR stack running
6. ⏳ Test failover scenarios
7. ⏳ Configure monitoring (Prometheus/Grafana)
8. ⏳ Setup backups (Proxmox Backup Server)

## Support

For detailed information, see:
- `HA-SETUP-GUIDE.md` - HA configuration
- `README-HA-ARR-STACK.md` - Full deployment guide
