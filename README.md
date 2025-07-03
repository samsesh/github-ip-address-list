# GitHub IP List for MikroTik

This repository automatically fetches the latest list of GitHub IP addresses from [https://api.github.com/meta](https://api.github.com/meta), and converts them into a MikroTik-compatible `.rsc` script file.

The script updates daily using GitHub Actions and is ideal for firewall rules, NAT exceptions, or routing GitHub traffic via specific interfaces.

---

## 📆 File Generated

* **`github-ip-list.rsc`**
  A MikroTik-compatible RouterOS script that adds all GitHub IP ranges into an address list named `GitHub`.

Example content:

```rsc
# Auto‎-generated MikroTik address list – GitHub IPs
/ip firewall address-list
add address=140.82.112.0/20 list=GitHub
add address=185.199.108.0/22 list=GitHub
add address=192.30.252.0/22 list=GitHub
...
```

---

## 🚀 MikroTik Import Script

To automatically fetch and import the IP list into your MikroTik router, run the following script:

```rsc
:foreach i in={"GitHub"} do={
  /tool fetch url="https://raw.githubusercontent.com/samsesh/github-ip-address-list/Localhost/github-ip-list.rsc" dst-path=$i
  /ip firewall address-list remove [/ip firewall address-list find list=$i]
  /import file-name=$i
  /file remove $i
}
```

✅ This script will:

* Download the latest IP list from your GitHub repo
* Remove old entries in the `GitHub` address list
* Import the updated list
* Clean up the downloaded file

---

## 🔁 Automation

This repo uses a [GitHub Actions workflow](.github/workflows/update-github-ips.yml) that:

* Runs daily at midnight UTC
* Fetches the latest GitHub IP ranges from the GitHub Meta API
* Builds and commits a MikroTik `.rsc` file (`github-ip-list.rsc`) to the `Localhost` branch

You can also run the workflow manually from the Actions tab.

---

## 📘 References

* GitHub API: [https://api.github.com/meta](https://api.github.com/meta)
* MikroTik Scripting Guide: [https://wiki.mikrotik.com/wiki/Manual\:Scripting](https://wiki.mikrotik.com/wiki/Manual:Scripting)
* MikroTik `/import` Command: [`/import file-name=yourfile.rsc`](https://wiki.mikrotik.com/wiki/Manual:Console#import)

---

## ☕ Support This Project

If you find this useful, you can support development here:
👉 [https://donate.samsesh.net](https://donate.samsesh.net)

---

## 🧑‍💻 Maintainer

**Author:** [samsesh](https://github.com/samsesh)
**Website:** [samsesh.net](https://samsesh.net)
