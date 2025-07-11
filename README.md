#pgzero
import random
WIDTH = 600
HEIGHT = 450

TITLE = "ikili oyun"
FPS = 30

# Nesneler
savas = Actor('bonus', (300, 200))
clicker = Actor('bonus', (300, 300))
#savaş
gemi = Actor("gemi", (300, 400))
uzay = Actor("uzay")
dusmanlar = []
gezegenler = [Actor("gezegen1", (random.randint(0, 600), -100)), Actor("gezegen2", (random.randint(0, 600), -100)), Actor("gezegen3", (random.randint(0, 600), -100))]
meteorlar = []
fuzeler = []
gemi1 = Actor("gemi", (100, 200))
gemi2 = Actor("gemi1", (300, 200))
gemi3 = Actor("gemi2", (500, 200))
puan1 = 0
o_mod = 'menü'
#clicker
hayvan = Actor("zürafa", (150, 250))
arkaplan = Actor("arkaplan")
bonus_1 = Actor("bonus", (450, 100))
bonus_2 = Actor("bonus", (450, 200))
bonus_3 = Actor("bonus", (450, 300))
oyna = Actor("oyna", (300, 100))
carpi = Actor("çarpı", (580, 20))
magaza = Actor("mağaza", (300, 200))
koleksiyon = Actor("koleksiyon", (300, 300))
timsah = Actor('timsah', (120, 200))
suaygiri = Actor('suaygırı', (300, 200))
denizayisi = Actor("denizayısı", (480, 200))

# Değişkenler
mod = "menü"
s_mod = "oyna"
puan = 0
tiklama = 1
ucret_1 = 15
ucret_2 = 200
ucret_3 = 600
hayvanlar = []
for i in range(5):
    x = random.randint(0, 600)
    y = random.randint(-450, -50)
    dusman = Actor("düşman", (x, y))
    dusman.speed = random.randint(2, 8)
    dusmanlar.append(dusman)
    
# Düşmanlar listesini oluşturmak
for i in range(5):
    x = random.randint(0, 600)
    y = random.randint(-450, -50)
    dusman = Actor("düşman", (x, y))
    dusman.speed = random.randint(2, 8)
    dusmanlar.append(dusman)
    
# Meteorlar listesini oluşturmak
for i in range(5):
    x = random.randint(0, 600)
    y = random.randint(-450, -50)
    meteor = Actor("meteor", (x, y))
    meteor.speed = random.randint(2, 10)
    meteorlar.append(meteor)

def draw():
    global puan1
    if o_mod == 'menü':
        arkaplan.draw()
        koleksiyon.draw()
        oyna.draw()
        magaza.draw()
        
    def on_mouse_down(button, pos):
        global puan
        global mod
        global ucret_1, ucret_2, ucret_3
        global tiklama
        global o_mod
        if o_mod == "oyun" or "magaza" or "koleksiyon" and carpi.collidepoint(pos):
                o_mod = 'menü'
        if mod == "oyun" and carpi.collidepoint(pos):
                o_mod = 'oyun'
        if button == mouse.LEFT and o_mod == "menü":
            if oyna.collidepoint(pos):
                o_mod = "oyun"
                carpi.draw()
                savas_draw()
                clicker.draw()
                screen.draw.text('clicker', center=(300, 300), color ="black", fontsize = 20)
                screen.draw.text('uzay savaşları', center=(300, 200), color ="black", fontsize = 20)
            if magaza.collidepoint(pos):
                timsah.draw()
                suaygiri.draw()
        if button == mouse.LEFT and o_mod == "oyun":
            if savas_collidepoint(pos):
                s_mod = "oyna"
            if clicker.collidepoint(pos):
                mod = "oyna"
        # Oyun Modu
        if button == mouse.LEFT and mod == "oyun":
            # Hayvanın üzerinde tıklama
            if hayvan.collidepoint(pos):
                puan += tiklama
                hayvan.y = 200
                animate(hayvan, tween='bounce_end', duration=0.5, y=250)
           # bonus_1 butonu tıklandığında  
            elif bonus_1.collidepoint(pos):
                bonus_1.y = 105
                animate(bonus_1, tween='bounce_end', duration=0.5, y=100)
                if puan >= ucret_1:
                    schedule_interval(bonus_1_icin, 2)
                    puan -= ucret_1
                    ucret_1 *= 2
                # bonus_2 butonu tıklandığında 
                elif bonus_2.collidepoint(pos):
                    bonus_2.y = 205
                    animate(bonus_2, tween='bounce_end', duration=0.5, )
                if puan >= ucret_2:
                    schedule_interval(bonus_2_icin, 2)
                    puan -= ucret_2
                    ucret_2 *= 2
            # bonus_3 button tıklandığında
            elif bonus_3.collidepoint(pos):
                bonus_3.y = 305
                animate(bonus_3, tween='bounce_end', duration=0.5, y=300)
                if puan >= ucret_3:
                    schedule_interval(bonus_3_icin, 2)
                    puan -= ucret_3
                    ucret_3 *= 2
            elif carpi.collidepoint(pos):
                mod = 'menü'
        # Menü Modu
        elif mod == 'menü' and button == mouse.LEFT:
            if oyna.collidepoint(pos):
                mod = 'oyun'
            elif magaza.collidepoint(pos):
                mod = 'mağaza'
            elif koleksiyon.collidepoint(pos):
                mod = "koleksiyon"
            
        # Mağaza        
        elif  mod == 'mağaza' and button == mouse.LEFT:
            if carpi.collidepoint(pos):
                mod = 'menü'
            # Timsahın Seçilmesi
            elif timsah.collidepoint(pos):
                timsah.y = 180
                animate(timsah, tween='bounce_end', duration=0.5)
                if puan >= 500:
                    puan -= 500
                    tiklama = 2
                    hayvan.image = 'timsah'
                    hayvanlar.append(timsah)
            # Su Aygırının Seçilmesi  
            elif suaygiri.collidepoint(pos):
                suaygiri.y = 180
                animate(suaygiri, tween='bounce_end', duration=0.5)
                if puan >= 2500:
                    puan -= 2500
                    tiklama = 3
                    hayvan.image = 'suaygırı'
                    hayvanlar.append(suaygiri)

            # denizayısının seçilmesi
            elif denizayisi.collidepoint(pos):
                denizayisi.y = 180
                animate(denizayisi, tween='bounce_end', duration=0.5,)
                if puan >= 7000:
                    puan -= 7000
                    tiklama = 4
                    hayvan.image = "denizayısı"
                    hayvanlar.append(denizayisi)


        # Koleksiyon
        elif  o_mod == 'koleksiyon' and button == mouse.LEFT:
            if carpi.collidepoint(pos):
                o_mod = 'oyun'
            # Timsahın Seçilmesi
            elif timsah.collidepoint(pos):
                timsah.y = 180
                animate(timsah, tween='bounce_end', duration=0.5)
                if timsah in hayvanlar:
                    hayvan.image = 'timsah'
                    tiklama = 2
                    
            # Su Aygırının Seçilmesi    
            elif suaygiri.collidepoint(pos):
                suaygiri.y = 180
                animate(suaygiri, tween='bounce_end', duration=0.5)
                if suaygiri in hayvanlar:
                    hayvan.image = 'suaygırı'
                    tiklama = 3

            # deniz ayısının seçilmesi
            elif denizayisi.collidepoint(pos):
                denizayisi.y = 180
                animate(denizayisi, tween='bounce_end', duration=0.5)
                if denizayisi in hayvanlar:
                    hayvan.image = "denizayısı"
                    tiklama = 4
   #clicker oyun
    if mod == 'oyun':    
        arkaplan.draw()
        hayvan.draw()
        screen.draw.text(puan, center=(150, 100), color="white", fontsize = 96)
        bonus_1.draw()
        screen.draw.text("Her 2 saniye için 1P", center=(450, 80), color="black", fontsize = 20)
        screen.draw.text(ucret_1, center=(450, 110), color="black", fontsize = 20)
        bonus_2.draw()
        screen.draw.text("Her 2 saniye için 15P", center=(450, 180), color="black", fontsize = 20)
        screen.draw.text(ucret_2, center=(450, 210), color="black", fontsize = 20)
        bonus_3.draw()
        screen.draw.text("Her 2 saniye için 50P", center=(450, 280), color="black", fontsize = 20)
        screen.draw.text(ucret_3, center=(450, 310), color="black", fontsize = 20)
        carpi.draw()
    
    elif o_mod == 'mağaza':
        arkaplan.draw()
        timsah.draw()
        screen.draw.text("500P", center= (120, 300), color="white", fontsize = 36)
        suaygiri.draw()
        screen.draw.text("2500P", center= (300, 300), color="white", fontsize = 36)
        denizayisi.draw()
        screen.draw.text("7000P", center = (480, 300), color = "white", fontsize = 36)
        carpi.draw()
        screen.draw.text(puan, center=(30, 20), color="white", fontsize = 36)
    
    elif o_mod == 'koleksiyon':
        arkaplan.draw()
        for i in range(len(hayvanlar)):
            hayvanlar[i].draw()
        carpi.draw()
        screen.draw.text(puan, center=(30, 20), color="white", fontsize = 36)
        screen.draw.text("+2P", center= (120, 300), color="white", fontsize = 36)
        screen.draw.text("+3P", center= (300, 300), color="white", fontsize = 36)
        screen.draw.text("+4P", center = (480, 300), color = "white", fontsize = 36)
    
    #savaş
    if s_mod == 'son':
        uzay.draw()
        screen.draw.text("OYUN BİTTİ!", center = (300, 200), color = "white", fontsize = 36)
        screen.draw.text(puan, center = (300, 250), color = "white", fontsize = 36)    
        screen.draw.text("tekrar oynamak için boşluk tuşuna basın", center = (300, 300), color = "white", fontsize = 30)
        if puan < 30:
            screen.draw.text("bu kötü oldu", center = (300, 150), color = "white", fontsize = 30)
            # Oyun Bitti ekranı

        # Kontroller

def on_mouse_move(pos):
    gemi.pos = pos

# Yeni Düşmanların Eklenmesi
def yeni_dusman():
    x = random.randint(0, 400)
    y = -50
    dusman = Actor("düşman", (x, y))
    dusman.speed = random.randint(2, 8)
    dusmanlar.append(dusman)

# Düşmanların Hareketleri
def dusman_gemisi():
    for i in range(len(dusmanlar)):
        if dusmanlar[i].y < 650:
            dusmanlar[i].y = dusmanlar[i].y + dusmanlar[i].speed
        else:
            dusmanlar.pop(i)
            yeni_dusman()

# Gezegenlerin Hareketleri
def gezegen():
    if gezegenler[0].y < 550:
            gezegenler[0].y = gezegenler[0].y + 1
    else:
        gezegenler[0].y = -100
        gezegenler[0].x = random.randint(0, 600)
        birinci = gezegenler.pop(0)
        gezegenler.append(birinci)

# Meteorların Hareketleri
def meteorlar_hareket():
    for i in range(len(meteorlar)):
        if meteorlar[i].y < 450:
            meteorlar[i].y = meteorlar[i].y + meteorlar[i].speed
        else:
            meteorlar[i].x = random.randint(0, 600)
            meteorlar[i].y = -20
            meteorlar[i].speed = random.randint(2, 10)

# Çarpışmalar
def carpismalar():
    global s_mod
    global puan
    for i in range(len(dusmanlar)):
        if gemi.colliderect(dusmanlar[i]):
            s_mod = 'son'

        # Füzelerin çarpışması
        for j in range(len(fuzeler)):
            if fuzeler[j].colliderect(dusmanlar[i]):
                puan = puan + 1
                dusmanlar.pop(i)
                fuzeler.pop(j)
                yeni_dusman()
                break
    def update(dt):
        if s_mod == 'oyun':
            dusman_gemisi()
            carpismalar()
            gezegen()
            meteorlar_hareket()
        # Füzelerin hareketleri
            for i in range(len(fuzeler)):
                if fuzeler[i].y < 0:
                    fuzeler.pop(i)
                    break

            else:
                fuzeler[i].y = fuzeler[i].y - 10
            
            
#clicker bonusları
def bonus_1_icin():
    global puan
    puan += 1

def bonus_2_icin():
    global puan
    puan += 15

def bonus_3_icin():
    global puan
    puan += 50


def on_key_down(key):
    global s_mod, puan
    if s_mod == 'son' and keyboard.space:
        s_mod = "menü"
        puan = 0
        dusmanlar.pop()
