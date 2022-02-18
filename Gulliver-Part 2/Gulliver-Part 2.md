# Gulliver—Part 2 (Zip Forensics)

You now direct your attention to the contents of the message you examined in Part 1 of this challenge.
Extract the critical business document contained within GOAT.zip in its entirety, and enter its MD5 hash.


*100 points*

## Approach

Referring to the prompt in the email:
>Hi there,<br>
I have been trying to extract a critical file from the attached ZIP. I've been told that the ZIP was recovered from a corrupt disk where some bytes within the first 24 bytes of the ZIP file could not be read. These unreadable bytes were replaced with zeros. Any chance you could extract the document inside the ZIP for me? Thx<br><br>
Ornat

So I though it had something to do with the `zip` file header. Trying to extract the original zip with 7zip or other utilities, we get an error. This could happen because the zip file header was in the first place not properly constructed, or tampered with.

We can first verify if the file is indeed a zip file with the `file` command.

```
C:\Users\user\Downloads
λ file GOAT.zip
GOAT.zip: Zip archive data, at least v2.0 to extract, compression method=deflate
```

While researching online, I came across [this article](https://users.cs.jmu.edu/buchhofp/forensics/formats/pkzip.html). Referring back to the email prompt, we can zoom in on the area to be modified.

![img]()

We can open the `zip` file in a hex editor such as HxD and through observing offsets `0x12 - 0x15`, we observe that the compressed size is almost zeroed while the uncompressed size field is populated.

![img]()

To resolve this discrepancy, we can simply convert the size from hex -> obtain this from the `Properties` tab in Windows Explorer using a calculator.

![img]()

Thereafter, we can modify the compressed file size with the appropriate one (note that it is stored in little endian). Doing so will enable the file to be unzipped and one to retrieve the PDF.

![img]()

We can verify that the contents of the zip by recalculating the uncompressed size again. 

We can then calculate and submit the MD5 sum.

![img]()
