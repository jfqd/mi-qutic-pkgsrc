# mi-qutic-pkgsrc

Use [joyent/mibe](https://github.com/joyent/mibe) to create a provisionable image

## description

SmartOS pkgsrc server.

## mdata variables

See [mi-qutic-base Readme](https://github.com/jfqd/mi-qutic-base/blob/master/README.md) for a list of usable metadata.

## installation

The following sample can be used to create an image server.

```
IMAGE_UUID=$(imgadm list | grep 'qutic-base-64' | tail -1 | awk '{ print $1 }')
vmadm create << EOF
{
  "brand":      "joyent",
  "image_uuid": "$IMAGE_UUID",
  "alias":      "pkgsrc-server",
  "hostname":   "pkgsrc.example.com",
  "dns_domain": "example.com",
  "resolvers": [
    "80.80.80.80",
    "80.80.81.81"
  ],
  "nics": [
    {
      "interface": "net0",
      "nic_tag":   "admin",
      "ip":        "10.10.10.10",
      "gateway":   "10.10.10.1",
      "netmask":   "255.255.255.0"
    }
  ],
  "max_physical_memory": 256,
  "quota":                 10,
  "cpu_cap":              100,
  "customer_metadata": {
    "admin_authorized_keys": "your-long-key",
    "root_authorized_keys":  "your-long-key",
    "mail_smarthost":        "mail.example.com",
    "mail_auth_user":        "you@example.com",
    "mail_auth_pass":        "smtp-account-password",
    "mail_adminaddr":        "report@example.com",
    "munin_master_allow":    "munin-master-ip",
    "admin_upload_token":    "your-secure-admin-token",
    "nginx_ssl":             "certificat-with-encoded-linebreaks",
    "vfstab":                "storage.example.com:/export/pkgsrc    -       /data    nfs     -       yes     rw,bg,intr"
  }
}
EOF
```
