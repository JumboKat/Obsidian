#Technology 

QR, or **Quick Response** is a 2-D barcode that is able to store information much more efficiently than a traditional barcode. They use a grid-like pattern to store data both horizontally and vertically. It was invented in 1994 by **Denso Wave**, a subsidiary of Toyota, and were originally used for tracking vehicle parts used in manufacturing. Their use has become widespread since.
### QR Structure
![[Pasted image 20260713113000.png]]
- **Finder Pattern**: Three large squares used by a scanner to identify the QR code. They contain a black center with a white ring and black border, and are located in the top-left, top-right, and bottom-left corners. The specific locations allow the scanner to figure out its orientation, regardless of the angle it is scanned from; only three points are required to establish orientation.
- **Alignment Pattern**: A smaller square in the bottom right corner used to help read the code accurately even if it is presented skewed, warped, or distorted.
- **Timing Pattern**: Lines of alternating black and white pixels between the finder patterns to help the scanner measure the dimensions of the QR code; acts like a ruler.
- **Version Information**: Located above the bottom finder pattern and left of the right finder pattern, these encoded bits specify the QR code's version (the version is essentially the size/grid density). Only appears in Version 7 and higher; smaller codes don't need a version label as it can be inferred from their size.
- **Quiet Zone**: A blank border around the whole code, at least four pixels wide, to help the scanner distinguish the code from its surroundings.
- **Data**: Where the actual information is encoded in black and white. Error correction keys are also stored so that the code can be read even if parts of it are damaged or dirty. The code can still function with up to 30% of the code destroyed. Because of this, they are ideal for outdoor information encoding.
The code's **version** determines its dimensions. Versions increase in steps of four modules/pixels per side (e.g. version 1 = 21x21, 2 = 25x25, 3 = 29x29, etc. up to version 40 at 177x177).
### Scanning
When a QR code is presented to a scanner, it must perform a series of actions to decode it into human-readable information.
1. Detection: the scanner locates the finder patterns to identify the code and understand its orientation.
2. Decoding: the scanner reads the timing and alignment patterns to determine its size and shape.
3. Interpretation: the scanner then parses the binary code stored in the data modules and translates this into usable information; either text, a URL, or performing an action.
### Storing Data
![[Pasted image 20260713114815.png]]
QR codes store information in binary, with each black square representing '1' and each white square representing '0.' The largest standard QR code (Version 40, or 177x177) can store a maximum of 2,953 bytes of data, which translates to 7089 numbers or 4,296 alphanumeric characters. 
### Rise in Popularity
In 2017, phone cameras integrated the ability to scan QR codes directly into their software, eliminating the need for dedicated third-party scanning apps. In 2020, the need for contactless information (payments, digital menus, virtual check-ins) during the COVID-19 pandemic caused a surge in the use of QR codes. 

While they are favoured for being quick and convenient, QR codes are also highly versatile, supporting many forms of information, such as:
- URLs
- Text
- Contact Information
- Payment links
- Software actions

![[Pasted image 20260713115805.png]]
Dynamic QR codes are an innovation upon the original QR code design. With older QR codes, the desired information was directly encoded into the QR code, and could not be changed afterward. Dynamic QR codes solve this issue by encoding an intermediate URL. This intermediate can then be altered to change the destination without changing the QR code itself. It also allows tracking statistics, like when, how often, and where it was scanned. The pattern also tends to be not as dense, making it easier for scanners to read.

---
### References
![](https://www.youtube.com/watch?v=IwVcvTZA_Do&list=PLsw0pFpRQcwAAWA42QsDcgtU4M5fUeG13&index=22)