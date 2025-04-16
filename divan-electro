import scrapy

class DivanLightingSpider(scrapy.Spider):
    name = 'divan_lighting'
    allowed_domains = ['divan.ru']
    start_urls = ['https://www.divan.ru/category/osveshchenie']

    def parse(self, response):
        # Ищем все товары в разделе освещения
        for product in response.css('div.swiper-slide.WaG4w._NtQG.H6c9z.ui-5nyfV'):
            price = product.css('span.ui-LD-ZU.OtETQ::text').get()
            if not price:
                price = product.css('span.ui-LD-ZU.ui-SVNym.emGSJ::text').get()
            
            yield {
                'name': product.css('a.ui-GPFV8.dM3t2.ProductName span::text').get().strip(),
                'price': price.replace('\xa0', ' ').strip() if price else None,
                'link': response.urljoin(product.css('a.ui-GPFV8::attr(href)').get())
            }

        # Пагинация: ищем ссылку на следующую страницу
        next_page = response.css('a[data-testid="pagination-button-next"]::attr(href)').get()
        if next_page:
            yield response.follow(next_page, callback=self.parse)
