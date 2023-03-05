```python
#Kütüphaneler

import csv
from datetime import datetime 
```


```python
#Menu.txt dosyası aç ve write metodu dosyayı her defasında sil ve baştan yaz kullan değişken adı pizzaMenu olsun 

def menu_yaz():
    with open("Menu.txt", "w") as pizzaMenu: 
        
        menu_icerik = """*Lütfen Bir Pizza Tabanı Seçiniz:
1: Klasik
2: Margarita
3: TürkPizza
4: Sade Pizza
* ve seçeceğiniz sos:
11: Zeytin
12: Mantarlar
13: Keçi Peyniri
14: Et
15: Soğan
16: Mısır
*Teşekkür ederiz!"""
        
        pizzaMenu.write(menu_icerik) 
        pizzaMenu.close() 
```


```python
menu_yaz() #Menu.txt İçin Fonksiyon Çağırma
```


```python
#Pizza Sınıfı Tanımlanır (Base & Price)

class Pizza:

    def __init__(self, description, cost): 
        self.description = description 
        self.cost = cost 

    def get_description(self):
        return self.description 

    def get_cost(self):
        return self.cost 
```


```python
#Pizza Sınıfından Miras Alan Alt Sınıflar

class klasikPizza(Pizza):

    def __init__(self):
        super().__init__("Klasik Pizza", 70.00)

class margaritaPizza(Pizza):

    def __init__(self):
        super().__init__("Margarita Pizza", 80.00)

class turkPizza(Pizza):
    
    def __init__(self):
        super().__init__("Turk Pizza", 90.00)

class sadePizza(Pizza):
    
    def __init__(self):
        super().__init__("Sade Pizza", 50.00)
```


```python
# super().__init__("Klasik Pizza", 70)

# class klasikPizza(Pizza):
#    def __init__(self):
#        self.description = "Klasik pizza"
#        self.cost = 70.0
```


```python
#Decorator Sınıfı

class Decorator(Pizza):

    def __init__(self, component, description, cost):
        self.component = component
        self.description = description
        self.cost = cost

    def get_description(self):
        return self.component.get_description() + " - Ekstralar: " + Pizza.get_description(self)

    def get_cost(self):
        return self.component.get_cost() + Pizza.get_cost(self)

    def get_saucecost(self):
        return self.cost
    
    def get_saucedescription(self):
        return self.description
```


```python
#Decoratorden Miras Alan Soslar

class zeytin(Decorator):
    
    def __init__(self, component):
        super().__init__(component, "Zeytin", 5.00)

class mantar(Decorator):

    def __init__(self, component):
        super().__init__(component, "Mantar", 7.50)

class keciPeynir(Decorator):
        
    def __init__(self, component):
        super().__init__(component, "Keci Peyniri", 15.00)

class et(Decorator):

    def __init__(self, component):
        super().__init__(component, "Et", 20.00)

class sogan(Decorator):
        
    def __init__(self, component):
        super().__init__(component, "Sogan", 2.50)    

class misir(Decorator):

    def __init__(self, component):
        super().__init__(component, "Misir", 10.00)
        
class noextra(Decorator):
    
    def __init__(self, component):
        super().__init__(component, "Extra Yok", 0.00)
```


```python
#ID Number

def isValidID(id_number):
    id_number = str(id_number) 

    if not len(id_number) == 11: #11 Karakter Olup Olmadığını Kontrol Edilir 
        return False
    
    if not id_number.isdigit(): #Sadece Rakam Olup Olmadığına Bakılır
        return False

    if int(id_number[0]) == 0: #ID Number 0 ile başlayamaz
        return False

    return True
```


```python
#Kredi Kartı

def isValidDebitCard(debitNo): 
    
    if len(debitNo) == 16: # KK 16 Haneli 
        return True   
```


```python
#Menu.txt Göster

def show_menu(): 

    with open('Menu.txt', 'r') as file:
        pMenu = file.read()
        print(pMenu)
```


```python
#Main Fonksiyon

def main():
    show_menu()
    
    while(True):
        pizzaBase = int(input("\nPizzanızı Seçiniz [1-4]: ")) #Pizza Tabanı Seçtirilir
        if pizzaBase == 1:
            pizza = klasikPizza()
            break
        elif pizzaBase == 2:
            pizza = margaritaPizza()
            break
        elif pizzaBase == 3:
            pizza = turkPizza()
            break
        elif pizzaBase == 4:
            pizza = sadePizza()
            break
        else:
            print("Hatalı Seçim ! Tekrar Giriş Yapınız.")
            
    orderBase = pizza #Pizza orderBase degiskenine kaydedilir.
    
    
    while(True):
        pizzaSauce = int(input("Eklenecek Ekstra Malzemeyi Seçiniz [11-16]:\n Ekstra İstemiyorsanız 00 Tuşlayın ")) 
        if pizzaSauce == 11:
            pizza = zeytin(pizza) 
            break
        elif pizzaSauce == 12:
            pizza = mantar(pizza)
            break
        elif pizzaSauce == 13:
            pizza = keciPeynir(pizza)
            break
        elif pizzaSauce == 14:
            pizza = et(pizza)
            break
        elif pizzaSauce == 15:
            pizza = sogan(pizza)
            break
        elif pizzaSauce == 16:
            pizza = misir(pizza)
            break
        elif pizzaSauce == 00:
            pizza = noextra(pizza)
            break
        else:
            print("Hatalı Seçim !, Tekrar Giriş Yapınız. ")

          
    ad = input("Adınız: ")
    id_number = input("Kimlik No: ")
    
    while(True):
        if isValidID(id_number): 
            break
        else:
            print("Hatalı No !, 11 Haneli Olmalıdır ve '0' ile Başlayamaz, Tekrar Deneyiniz.")
            id_number = input("Kimlik No: ")
    
    print("\nSiparişiniz: ")
    print("Pizzanız: " + orderBase.get_description() + ' - ' + 'Ekstra Malzeme: ' + pizza.get_saucedescription()) 
    totalCost = orderBase.get_cost() + pizza.get_saucecost()
    print("Toplam Tutar: " + f"{totalCost} ₺" )
    
    kkNo = input("Ödeme için Kredi Kart No Giriniz: ")
    
    while(True):
        if isValidDebitCard(kkNo):
            break
        else:
            print("Hatalı Giriş, Tekrar Deneyiniz. KK No 16 Haneli Olmalıdır.")
            kkNo = input("Ödeme için Kredi Kart No Giriniz: ")
            
    kkPass = input("Şifrenizi Giriniz: ")
    
    description = pizza.get_description()
    cost = pizza.get_cost()
    now = datetime.now()
    dateTime = now.strftime("%d/%m/%Y, %H:%M:%S")
    
        
    print("\nTeşekkürler!")
    print("\nSipariş Detayları:")
    print("AD: " + ad)
    print("ID No: " + id_number)
    print("Sipariş: " + description)
    print("Cost:" + f"{cost} ₺")
    print("Sipariş Zamanı: " + dateTime)
    print("Bilgiler Kaydedildi.!")
 

    with open("Orders_Database.csv", "a") as order_info: #CSV Database Yaratma
        database = csv.writer(order_info) 
        database.writerow([ad, id_number, kkNo, description, cost, dateTime, kkPass])
                
            
```


```python
main()
```

    *Lütfen Bir Pizza Tabanı Seçiniz:
    1: Klasik
    2: Margarita
    3: TürkPizza
    4: Sade Pizza
    * ve seçeceğiniz sos:
    11: Zeytin
    12: Mantarlar
    13: Keçi Peyniri
    14: Et
    15: Soğan
    16: Mısır
    *Teşekkür ederiz!



```python

```
