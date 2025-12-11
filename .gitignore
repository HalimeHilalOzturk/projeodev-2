#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <stdbool.h>

// --- SABİT TANIMLAMALAR ---
#define SATIR 9        // Tahtanın satır sayısı
#define SUTUN 9        // Tahtanın sütun sayısı
#define MAYIN_SAYISI 10 // Tahtaya yerleştirilecek mayın sayısı

// --- GLOBAL TAHTALAR ---
// Gerçek Tahta: Mayınları ve komşu sayılarını tutar.
// -1: Mayın, 0-8: Komşu Mayın Sayısı
int gercekTahta[SATIR][SUTUN]; 

// Görüntü Tahtası: Oyuncunun gördüğü durumu tutar.
// 0: Gizli (#), 1: Açık (Sayı/Boş)
int goruntuDurumu[SATIR][SUTUN]; 

// --- FONKSİYON PROTOTİPLERİ ---
void tahtayiBaslat();
void mayinlariYerlestir();
void komsuSayilariniHesapla();
void tahtayiGoster(bool tumuAcik);
void otomatikAcma(int r, int c); // Recursive (Yinelemeli) fonksiyon
bool hamleYap(int r, int c);
bool kazanmaKontrol();

// ----------------------------------------------------------------------
// ANA FONKSİYON: Oyunun akışını yönetir.
// ----------------------------------------------------------------------

int main() {
    int satir, sutun;
    bool oyunBitti = false;
    bool kaybetti = false;

    // Rastgele sayı üreteci için tohumu ayarla
    srand(time(0)); 

    // Tahta kurulumu
    tahtayiBaslat();
    mayinlariYerlestir();
    komsuSayilariniHesapla();
    
    printf("--- BASİTLEŞTİRİLMİŞ MAYIN TARLASI (%dx%d, %d mayın) ---\n", SATIR, SUTUN, MAYIN_SAYISI);

    // Ana Oyun Döngüsü
    while (!oyunBitti) {
        tahtayiGoster(false); // Gizli tahtayı oyuncuya göster

        printf("\nHamlenizi Girin (Satır Sütun, ör: 3 5): ");
        if (scanf("%d %d", &satir, &sutun) != 2) {
            printf("Hatalı giriş. Lütfen sadece sayı girin.\n");
            while (getchar() != '\n'); 
            continue;
        }

        // 1-tabanlı girişi, 0-tabanlı indekse çevir
        satir--; 
        sutun--;

        // Geçersiz koordinat kontrolü
        if (satir < 0 || satir >= SATIR || sutun < 0 || sutun >= SUTUN) {
            printf("Hatalı koordinat.\n");
            continue;
        }

        // Hamleyi işleme (Mayına basıldıysa oyun biter)
        if (hamleYap(satir, sutun) == false) {
            kaybetti = true;
            oyunBitti = true;
        } else {
            // Kazanma kontrolü
            if (kazanmaKontrol()) {
                oyunBitti = true;
            }
        }
    }

    // Oyun sonu mesajı ve tahtanın tümünü göster
    printf("\n\n--- OYUN SONU ---\n");
    tahtayiGoster(true); 

    if (kaybetti) {
        printf("\n?? MAYINA BASTINIZ! OYUN BİTTİ. ??\n");
    } else {
        printf("\n?? TEBRİKLER! KAZANDINIZ! ??\n");
    }

    return 0;
}

// ----------------------------------------------------------------------
// YARDIMCI VE KURULUM FONKSİYONLARI
// ----------------------------------------------------------------------

// Tahtaları başlangıç değerlerine ayarlar
void tahtayiBaslat() {
    for (int i = 0; i < SATIR; i++) {
        for (int j = 0; j < SUTUN; j++) {
            gercekTahta[i][j] = 0;   // Mayınsız başlangıç
            goruntuDurumu[i][j] = 0; // Gizli başlangıç
        }
    }
}

// Tahtaya rastgele mayınları yerleştirir
void mayinlariYerlestir() {
    int mayinKoyuldu = 0;
    while (mayinKoyuldu < MAYIN_SAYISI) {
        int r = rand() % SATIR; 
        int c = rand() % SUTUN; 

        if (gercekTahta[r][c] != -1) {
            gercekTahta[r][c] = -1; // -1: Mayını temsil eder
            mayinKoyuldu++;
        }
    }
}

// Mayınların etrafındaki komşu sayısını hesaplar
void komsuSayilariniHesapla() {
    for (int i = 0; i < SATIR; i++) {
        for (int j = 0; j < SUTUN; j++) {
            if (gercekTahta[i][j] != -1) {
                int sayac = 0;
                // 8 komşuyu kontrol etmek için döngüler
                for (int dr = -1; dr <= 1; dr++) {
                    for (int dc = -1; dc <= 1; dc++) {
                        if (dr == 0 && dc == 0) continue; // Kendi hücresi değilse

                        int nr = i + dr; 
                        int nc = j + dc; 

                        // Tahta sınırları içinde mi?
                        if (nr >= 0 && nr < SATIR && nc >= 0 && nc < SUTUN) {
                            if (gercekTahta[nr][nc] == -1) {
                                sayac++; // Komşu mayın
                            }
                        }
                    }
                }
                gercekTahta[i][j] = sayac;
            }
        }
    }
}

// Tahtayı ekrana formatlı bir şekilde bastırır.
void tahtayiGoster(bool tumuAcik) {
    // 
    // ... (Kodda detaylı tahta gösterme mantığı yer alıyor)
    int i, j;
    // (Sütun ve satır indeksleri basılır)
    printf("\n    ");
    for (j = 0; j < SUTUN; j++) { printf("%2d ", j + 1); }
    printf("\n   ");
    for (j = 0; j < SUTUN * 3; j++) { printf("-"); }
    printf("\n");

    for (i = 0; i < SATIR; i++) {
        printf("%2d |", i + 1); 
        for (j = 0; j < SUTUN; j++) {
            if (tumuAcik) { // Oyun sonu
                if (gercekTahta[i][j] == -1) { printf(" * "); } // Mayın
                else if (gercekTahta[i][j] == 0) { printf(" . "); } // Boş
                else { printf(" %d ", gercekTahta[i][j]); } // Sayı
            } else { // Oyun devam ediyor
                if (goruntuDurumu[i][j] == 1) { // Açık
                    if (gercekTahta[i][j] == 0) { printf(" . "); }
                    else { printf(" %d ", gercekTahta[i][j]); }
                } else {
                    printf(" # "); // Gizli
                }
            }
        }
        printf("|\n");
    }
    printf("   ");
    for (j = 0; j < SUTUN * 3; j++) { printf("-"); }
    printf("\n");
}

// Rekürsif (Yinelemeli) fonksiyon: Boş (0 komşulu) bir hücre açıldığında etrafını otomatik açar.
void otomatikAcma(int r, int c) {
    // Durma Koşulları (Sınır kontrolü)
    if (r < 0 || r >= SATIR || c < 0 || c >= SUTUN) { return; }
    if (goruntuDurumu[r][c] == 1) { return; } // Zaten açık

    // Hücreyi aç
    goruntuDurumu[r][c] = 1;

    // Eğer hücrede sayı varsa, rekürsif yayılmayı durdur.
    if (gercekTahta[r][c] > 0) { return; } 
    
    // Hücre 0 ise, 8 komşusunu kontrol et ve recursive çağrı yap
    for (int dr = -1; dr <= 1; dr++) {
        for (int dc = -1; dc <= 1; dc++) {
            if (dr == 0 && dc == 0) continue; // Kendi hücresi değilse
            otomatikAcma(r + dr, c + dc); // Recursive çağrı
        }
    }
}

// Oyuncunun yaptığı hamleyi işler. Mayına basılırsa false döner (kaybetti).
bool hamleYap(int r, int c) {
    // 1. Mayın kontrolü
    if (gercekTahta[r][c] == -1) {
        return false; // Kaybetti
    }

    // 2. Hamleyi işleme
    if (gercekTahta[r][c] == 0) {
        otomatikAcma(r, c); // Otomatik açmayı başlat
    } else {
        goruntuDurumu[r][c] = 1; // Sadece bu hücreyi aç
    }
    
    return true; // Güvenli hamle
}

// Kazanma durumunu kontrol eder
bool kazanmaKontrol() {
    int acilmamisSayisi = 0;

    for (int i = 0; i < SATIR; i++) {
        for (int j = 0; j < SUTUN; j++) {
            // Sadece gizli hücreleri say
            if (goruntuDurumu[i][j] == 0) {
                acilmamisSayisi++;
            }
        }
    }

    // Gizli hücre sayısı, toplam mayın sayısına eşitse kazanılır.
    return (acilmamisSayisi == MAYIN_SAYISI);
}
