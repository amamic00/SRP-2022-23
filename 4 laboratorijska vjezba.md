# 4. laboratorijska vjezba

Cilj nam je bio zastititi integritet tekstualne datoteke, njenog sazdrzaja. Osigurali smo autenticnost primjenom MACa. Zapoceli smo tako sto smo unutar foldera u kojem radimo kreirali datoteku, dodali jos jedan file gdje smo implementirali zastitu integriteta. 

1.dio - mi smo netko tko salje poruku 

2.dio - osiguravamo autenticnost nekog tko verificira

Korak 1.1 prilikom potpisivanja filea je otvoriti file (Read file content), nakon toga potpisati file i krajnji cilj je to poslati nekoj drugoj strani, Treci korak bio je spremiti potpis u file. 

Druga strana to prima, ucita sadrzaj primljenog filea, potpis, lokalno treba potpisat file i usporediti potpise te lokalno generirat s onim primljenim. 

```python
import datetime
import re 
from pathlib import Path
from cryptography.hazmat.primitives import hashes, hmac
from cryptography.exceptions import InvalidSignature

def generate_MAC(key, message):
    if not isinstance(message, bytes):
        message = message.encode()

    h = hmac.HMAC(key, hashes.SHA256())
    h.update(message)
    signature = h.finalize()
    return signature

def verify_MAC(key, signature, message):
    if not isinstance(message, bytes):
        message = message.encode()

    h = hmac.HMAC(key, hashes.SHA256())
    h.update(message)
    try:
        h.verify(signature)
    except InvalidSignature:
        return False
    else:
        return True

if __name__ == "__main__":

# 1. Sign the file content
# 1.1 Reading file content
      with open("message.txt", "rb") as file:
          content = file.read()  
   
# # 1.2 Sign the content
 key= "my super secure secret".encode()
 signature=generate_MAC(key=key, message=content)
 print(signature)

 # 1.3 Save the signature into a file
 with open("message.sig", "wb") as file:
         file.write(signature)  

# 2. Verify message authenticy
# 2.1 Read the received file  
with open("message.sig", "rb") as file:
    signature = file.read()

# 2.2 Read the received signature
 with open("message.sig", "rb") as file:
        signature = file.read()

# 2.3.1 Sign the received file
# 2.3.2 Compare locally generated signature with the recieved one
 key= "my super secure secret".encode()
 is_authentic = verify_MAC(key=key, signature=signature, message=content)
 print(f"Message is {'OK' if is_authentic else  'NOK'}")
  
    PATH="challenges/g2/mamic_anamarija/mac_challenge/"
    KEY="mamic_anamarija".encode()
    authentic_messages = []
    for ctr in range(1, 11):
        msg_filename = f"order_{ctr}.txt"
        sig_filename = f"order_{ctr}.sig"    
       
        msg_file_path = Path(PATH + msg_filename)
        with open(msg_file_path, "rb") as file:
            message= file.read()

        sig_file_path = Path(PATH + sig_filename)
        with open(sig_file_path, "rb") as file:
            signature= file.read()
       
        is_authentic = verify_MAC(key=KEY, signature=signature, message=message)
        # print(f'Message {message.decode():>45} {"OK" if is_authentic else "NOK":<6}')
        if is_authentic:
            authentic_messages.append(message.decode())
        print(authentic_messages)

        authentic_messages.sort(
            key=lambda m: datetime.datetime.fromisoformat(
                re.findall(r'\(.*?\)', m)[0][1:-1]
            )
        )

        for m in authentic_messages:
                print(f'Message {m:>45 } {"OK:<6"}')
```