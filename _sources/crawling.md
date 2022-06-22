# Crawling Data

Crawling data adalah prosedur pengumpulan data besar yang dapat menjelajah hingga ke halaman paling dalam. Untuk crawling data menggunakan salah satu framework python yaitu scrapy. Scrapy menyediakan segala tools yang kita butuhkan untuk mengekstrak data dari setiap website secara efisien, memprosesnya, lalu menyimpannya dalam struktur atau format yang kita inginkan.

## Instalasi Scrapy

Untuk menginstall scrapy dapat menggnakan perintah pip di Command Prompt berikut:

```python
pip install scrapy
```

## Membuat File Scrapy

Setelah selesai menginstall Scrapy, maka buat file scrapy baru dengan kode:

```python
scrapy startproject namaproject
```

## Menjalankan Spyder baru

Yang harus dilakukan, masuk ke dalam projectscrapy kemudian membuat spider baru dengan kode:

```python
cd nama project
```

Setelah menjalankan spyder baru, tuliskan command yang isinya nama file dan alamat website yang akan diambil datanya agar lebih mudah saat akan membuat.

```python
scrapy genspider example example.com
```

## Menulis Program Scapper

Setelah itu, pada pembuatan spyder file yang tadi telah dibuat telah tersedia dengan nama file yang telah dibuat.

Pada file yang telah dibuat akan ada code untuk melakukan scraping yang dapat diubah sesuai keinginan

```python
import scrapy

class scrapPTA(scrapy.Spider):
    name = 'PTA'
    allowed_domains = ['pta.trunojoyo.ac.id']
    start_urls = ['https://pta.trunojoyo.ac.id/c_search/byprod/10/'+str(x)+" " for x in range(2,20)]

    def parse(self, response):
        for link in response.css('a.gray.button::attr(href)') :
            yield response.follow(link.get(),callback=self.parse_categories)

    def parse_categories(self, response):
        products = response.css('div#content_journal ul li')
        for product in products:
            yield {
                'judul' : product.css('div a.title::text').get().strip(),
                'penulis' : product.css('div div:nth-child(2) span::text').get().strip(),
                'dosen 1' : product.css('div div:nth-child(3) span::text').get().strip(),
                'dosen 2' : product.css('div div:nth-child(4) span::text').get().strip(),
                'abstrak' : product.css('div div:nth-child(2) p::text').get().strip()
            }
```



## Menjalankan File Spider

Pertama, masuk kedalam direktori spider dulu. setelah itu, jalankan spider dengan command 

```python
scrapy runspider namafile.py
```

## Menyimpan data kedalam excel

Data yang telah diambil dari suatu web dapat disimpan dengan kode:

```python
scrapy crawl namafile -O namafileyangdiinginkan.xlsx
```

