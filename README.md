# xiaomi air Purifier reverse engineering
Xiaomi air Purifier H3 reverse engineering

# Please read the wiki first!


The sole purpose of the information being released is to learn/develop a opensource alternative to the chinese firmware that the devices are currently using.


Blog Post Containing Background information:

post 1 : [Click](https://flamingo-tech.nl/2021/03/12/xiaomi-air-purifier-3h-reverse-engineering-part1/)

post 2 : [Click](https://flamingo-tech.nl/2021/04/05/xiaomi-air-purifier-3h-reverse-engineering-part-2-fremont-dump/)

post 3 : [Click](https://flamingo-tech.nl/2021/05/03/xiaomi-air-purifier-3h-reverse-engineering-part-3-esp32-dump/)

```
import sys
import hashlib

Usage: pwd.py 04A03CAA1E7080
def getpwd(uid):
    uid = bytearray.fromhex(uid)
    h = bytearray.fromhex(hashlib.sha1(uid).hexdigest())
    pwd = ""
    pwd += "%02X" % h[h[0] % 20]
    pwd += "%02X" % h[(h[0]+5) % 20]
    pwd += "%02X" % h[(h[0]+13) % 20]
    pwd += "%02X" % h[(h[0]+17) % 20]
    return pwd

assert getpwd("04A03CAA1E7080") == "CD91AFCC"
assert getpwd("04112233445566") == "EC9805C8"
print("PWD:", getpwd(sys.argv[1]))
```

To most of you this might look like some random code.. But this is actually very special… Xiaomi relies on a password for communication between filter and air purifier. More information can be found on the reverse engineering Github: Click

The type of NFC tags that are used are the NTAG213 tags (by NXP) How they created the password was a secret until now!

They use the UUID (duhh its, unique for each filter) If we use the above code and and insert a filter with UUID : 04A03CAA1E7080 we get the password CD91AFCC.


