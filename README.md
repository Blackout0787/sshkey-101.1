ssh-keygen -t rsa -b 4096 -m PEM
raw = File.binread "cert.cer" # DER- or PEM-encoded
cert = ...
File.open("cert.pem", "wb") { |f| f.print cert.to_pem }

```ruby
f = File.read(File.expand_path("~/.ssh/$ cat ~/.ssh/id_rsa.pub"))
k = SSHKey.new(f, comment: "blackout071987@gmail.com")
```

If your existing key is in the OpenSSH format (starts with `-----BEGIN OPENSSH PRIVATE KEY-----`), you'll need to convert it to PEM format or generate a new key.

Generate a new RSA key in PEM format with `ssh-keygen`:

```
ssh-keygen -t rsa -b 4096 -m PEM
```

Or convert an existing OpenSSH-formatted key with the following. This will modify the existing private key file.

```
ssh-keygen -p -N "" -m pem -f /path/to/existing/private/key
```

### The SSHKey object

#### Private and public keys
@@ -159,7 +173,7 @@ puts k.randomart

#### Original OpenSSL key object

Return the original [OpenSSL::PKey::RSA](https://ruby-doc.org/3.2.2/exts/openssl/OpenSSL/PKey/RSA.html) or [OpenSSL::PKey::DSA](https://ruby-doc.org/3.2.2/exts/openssl/OpenSSL/PKey/DSA.html) or [OpenSSL::PKey::EC](https://ruby-doc.org/3.2.2/exts/openssl/OpenSSL/PKey/EC.html)object.
Return the original [OpenSSL::PKey::RSA](https://ruby-doc.org/3.2.2/exts/openssl/OpenSSL/PKey/RSA.html) or [OpenSSL::PKey::DSA](https://ruby-doc.org/3.2.2/exts/openssl/OpenSSL/PKey/DSA.html) or [OpenSSL::PKey::EC](https://ruby-doc.org/3.2.2/exts/openssl/OpenSSL/PKey/EC.html) object.

```ruby
k.key_object
# => -----BEGIN RSA PRIVATE KEY-----\nMIIEowI...
```
### Existing SSH public keys
#### Validation
Determine if a given SSH public key is valid. Very useful to test user input of public keys to make sure they accurately copy/pasted the key. Just pass the SSH public key as a string. Returns false if the key is invalid.
```ruby
SSHKey.valid_ssh_public_key? "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9HuXvYJPtQE/o/7TYi63yAopsrJ6TP+lDGdyQ+nVVp+5ojAIy9h8/h99UlNxjkiFT2YhI3Fl/pgNDRO4PVo6tlgb3CwiAZjSdeE5RnF79Dkj5XsM4j+FLMoXtbRw0K9ok9RKjz6ygIs1JDmaOdXexFnq4nAYU3fSLUa6WoccqTHe8bFuJoAv1gbnx09Js8YcVMD96mpTJ3V/MK5YfIv10dbtrDhGug3IS1V2J+0BB9orbQja554N+4S0I9rFBgVCpvPmQqddDHd/AdGkLv/zjEfGytjnvp68bEfDinkQkPfuxw01yd5MbcvLv39VVICWtKbqW263HT5LvSxwKorR7"
# => true
```
#### Bit length
Determine the strength of the key in bits as an integer. Returns `SSHKey::PublicKeyError` if bits cannot be determined.
```ruby
SSHKey.ssh_public_key_bits "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9HuXvYJPtQE/o/7TYi63yAopsrJ6TP+lDGdyQ+nVVp+5ojAIy9h8/h99UlNxjkiFT2YhI3Fl/pgNDRO4PVo6tlgb3CwiAZjSdeE5RnF79Dkj5XsM4j+FLMoXtbRw0K9ok9RKjz6ygIs1JDmaOdXexFnq4nAYU3fSLUa6WoccqTHe8bFuJoAv1gbnx09Js8YcVMD96mpTJ3V/MK5YfIv10dbtrDhGug3IS1V2J+0BB9orbQja554N+4S0I9rFBgVCpvPmQqddDHd/AdGkLv/zjEfGytjnvp68bEfDinkQkPfuxw01yd5MbcvLv39VVICWtKbqW263HT5LvSxwKorR7"
SSHKey.valid_ssh_public_key? "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9HuXvYJPtQE/o/7TYi63yAopsrJ6TP+lDGdyQ+nVVp+5ojAIy9h8/h99UlNxjkiFT2YhI3Fl/pgNDRO4PVo6tlgb3CwiAZjSdeE5RnF79Dkj5XsM4j+FLMoXtbRw0K9ok9RKjz6ygIs1JDmaOdXexFnq4nAYU3fSLUa6WoccqTHe8bFuJoAv1gbnx09Js8YcVMD96mpTJ3V/MK5YfIv10dbtrDhGug3IS1V2J+0BB9orbQja554N+4S0I9rFBgVCpvPmQqddDHd/AdGkLv/zjEfGytjnvp68bEfDinkQkPfuxw01yd5MbcvLv39VVICWtKbqW263HT5LvSxwKorR7"
# => true