# hCaptcha Challenger

The [hCaptcha Challenger](https://github.com/QIN2DIM/hcaptcha-challenger) is 🥂 Gracefully face hCaptcha challenge with YOLOv6(ONNX) embedded solution.

## Supported Languages

- `zh`: Chinese
- `en`: English

## Supported Task Types

- `image_label_binary`: Image Label Binary Task

## APIs

HOST: `http://captcha-challenger.herokuapp.com`

Method: `POST`

API: `/api/hcaptcha/`

Data:

| Key         | Type     | Description                                                              |
| ----------- | -------- | ------------------------------------------------------------------------ |
| `challenge` | `string` | The challenge task, e.g. `image_label_binary`                            |
| `prompt`    | `string` | The prompt task, e.g. `Please click each image containing a living rοom` |
| `lang`      | `string` | The language, e.g. `en`                                                  |

File:

| Key   | Type   | Description              |
| ----- | ------ | ------------------------ |
| `img` | `file` | The challenge image file |

## Examples

### Python

```python
import os
import requests

if __name__ == "__main__":
    challenge = "image_label_binary"
    prompt = "Please click each image containing a living rοom"
    img_urls = [
        "https://imgs.hcaptcha.com/GPEoiU+R9bUXTNqmcwQY3SethKUsfWLJUvigkUD1WIz7ShysOtrNzEQ6FtVGfxEkcPhyx4FSj+6URKFDVDo1yvejDFnQs6JYpP+/5VaY0iuc0VCk+XoKGiDU/vXeinlX+m3T+b+wvBeSz7hmdHLPARa6zeqgEMKb8fzODgSYGHjNmHBrwZKEVsqOZW4=/5xjvDyzFXjOozzB",
        "https://imgs.hcaptcha.com/N78vELA57x71P7xMfQ9rsKlwj7KWVWA823On41aBjRYeDycS/ObU4ZjXyfHXrUUZv/ZRDaupV1rFs1ZgKmhnhi+gmVJJ+pz6gLvoT1jAUOfGg69pF1Cy4Ct5QNVnbhyD1iYpfDdE0FrESqcZ1iRkY62EouI+wTWqzpa+EE5J+Btz8Zf72YyaWVdlUPg=44a04At4xqm458/p",
        "https://imgs.hcaptcha.com/1pSEL6FfhP8ZszEeqKfvTakQqzAdUJJeNACH3Sk9BxLz7AGFmmPQCF8OL6sE2UPHxecWO7ZaHrI/sPEcy1HPwdpvHJBNizaimOMcVW8KW9jzsSaU+8hExQuH07wXx3uzfADDYnnbMAVa89qf7SzL1pxp4KAjgyePJJuwexNBzzjelcVeWBhApRJtJMw=DI98ApAqjWBuMCZk",
        "https://imgs.hcaptcha.com/SI6hyPmNadJl8IDnnZsU7UolOuEOxeGRwqXJtWS/uSiUpA6/GnaePfbTugFZPyAyNa0c5iBnoimpnPh5DwVixLK6JIgo36sEMF15FdF5Xz4t8I5EN4o16CpKPOJLicAdaj3lVpvfPebpT3gzt95L0BYZed8Dg+3fxLL0/yVHb+H0AXMJiYIZXJvbfJI=qbqFvplUh4e9e29g",
        "https://imgs.hcaptcha.com/BdpUMo+zXw+aeAZS+pMrDZe0eD953rByIMJBZWdqt5HpIXmH4ec2DNL5szlLDA9Tfbw3JjYU/AoSegO89WGTja1Rgux26ieD3VcKC92Bbe57hujVh/cKECvBAeqb1Zc5OqtrUXZeESzTEPanUELoNO89A6eACbxeDKMJg3Z8I1PWyD/Vt25Yb6nEiT0=EFTyypD8F4x3toNu",
        "https://imgs.hcaptcha.com/9jm+97oAb1yMTYCe9F+JFXWzX8iyWeUA1qax56bPI3+douDzOF8Wa/hY0cJ6ZD3HETpjs6pAzur4CBnNoWKEKFoIKI0D5zjpD9uAGdohG0nZAB1vUwSkUDVoWFYQJq01HGe9Dm8DGJXl40blNSVRiPkUy86JbKFILKJ1o8KO3ybdQrBHrto0kImF/NI=unPpVZ/Eq3mV9Fw7",
        "https://imgs.hcaptcha.com/pAPADykomPse2XzdBpS5cfi6okZkgfGkOpuiB4znulKcuk4nvmlQ3kKHxxkWyGnDwpqW3zrA16whnvD6tiL8RYh0SBXcAeM+/VHS/TytRzMuJGRMi28cN+zXE0BNLnH3JhG1GC/nX7e1iys167oUBazroIn78IbJVTm6C8FdbvIrXnttHLgwX4AcgcI=PsgmVZvN6rsM6QNR",
        "https://imgs.hcaptcha.com/UKlvboNhnBTe9yElmpqnwxku0J9/Z4NbSVMt7C7nkcGX9XtE9W37kLT6rmqAXBCyrjq75O6TgB2tUH+K3cxkpjMr9r34mobHtFY1e+3077zTsIUcuFedSF1mjZQOlE7CX5gNskHD2iGX1RrV+KJEg/Evx2HvIaPHm7W7Hl73gzVgAYLZVbQGsfLYIXs=ZRHGGWSGBQFI4K74",
        "https://imgs.hcaptcha.com/EX915sHtBBY6h18SzZFe/BGyryvajff5pYbhsETEU49n8gJHuoGkyx/TYzhP3QgUW/ZfAoW5mc27oD6HfkpLGHNUxuGqQtD8sTnJWthSsWrASpzwVPCi+T5qzSBHPxj3eHUx1WKM10pzOA+X06vUr7ROSyepboL9rKlw2p2OwmSoMCLtn3trQdmSnB0=UJx3T+ODP1EYdlKP",
    ]
    img_dir = os.path.join(os.path.dirname(os.path.abspath(__file__)), "imgs")
    if not os.path.exists(img_dir):
        os.makedirs(img_dir)

    # fixme: you need to download the images and put them in the imgs directory

    # upload all images by post method
    files = []
    for i, url in enumerate(img_urls):
        img_name = os.path.join(img_dir, "{}.png".format(i + 1))
        files.append(("img", open(img_name, "rb")))

    host = "http://captcha-challenger.herokuapp.com"
    # host = "http://localhost:8000/"
    url = f"{host}/api/hcaptcha/"
    data = {
        "challenge": challenge,
        "prompt": prompt,
        "lang": "en",
    }
    result = requests.post(url, data=data, files=files)

    print(result.status_code)
    print(result.text)

```
