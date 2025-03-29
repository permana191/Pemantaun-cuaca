# Pemantaun-cuaca
import datetime
from collections import defaultdict

class WeatherMonitor:
    def __init__(self):
        self.weather_data = {}
        
    def input_weather_data(self):
        """Fungsi untuk memasukkan data cuaca harian"""
        date = input("Masukkan tanggal (YYYY-MM-DD) atau kosongkan untuk hari ini: ")
        if not date:
            date = str(datetime.date.today())
        
        try:
            temperature = float(input("Masukkan suhu (Celsius): "))
            humidity = float(input("Masukkan kelembaban (%): "))
            condition = input("Masukkan kondisi cuaca (cerah/hujan/berawan): ").lower()
            
            if condition not in ['cerah', 'hujan', 'berawan']:
                print("Kondisi cuaca tidak valid. Harap masukkan cerah/hujan/berawan.")
                return
                
            self.weather_data[date] = {
                'suhu': temperature,
                'kelembaban': humidity,
                'kondisi': condition
            }
            print(f"Data cuaca untuk tanggal {date} berhasil disimpan!")
            
        except ValueError:
            print("Input tidak valid. Pastikan suhu dan kelembaban berupa angka.")
    
    def display_recent_weather(self, days=5):
        """Menampilkan data cuaca beberapa hari terakhir"""
        if not self.weather_data:
            print("Belum ada data cuaca yang tersimpan.")
            return
            
        sorted_dates = sorted(self.weather_data.keys(), reverse=True)
        recent_dates = sorted_dates[:days]
        
        print("\nData Cuaca Beberapa Hari Terakhir:")
        print("-" * 50)
        print(f"{'Tanggal':<12} | {'Suhu (C)':<8} | {'Kelembaban (%)':<12} | {'Kondisi':<10} | Rekomendasi")
        print("-" * 50)
        
        for date in recent_dates:
            data = self.weather_data[date]
            recommendation = self.get_recommendation(data['kondisi'])
            print(f"{date:<12} | {data['suhu']:<8.1f} | {data['kelembaban']:<12.1f} | {data['kondisi']:<10} | {recommendation}")
    
    def weather_trends(self):
        """Menampilkan tren cuaca dari data yang tersimpan"""
        if len(self.weather_data) < 2:
            print("Minimal butuh 2 hari data untuk melihat tren.")
            return
            
        sorted_dates = sorted(self.weather_data.keys())
        
        # Hitung rata-rata
        avg_temp = sum(data['suhu'] for data in self.weather_data.values()) / len(self.weather_data)
        avg_humidity = sum(data['kelembaban'] for data in self.weather_data.values()) / len(self.weather_data)
        
        # Hitung kondisi cuaca yang paling sering muncul
        condition_counts = defaultdict(int)
        for data in self.weather_data.values():
            condition_counts[data['kondisi']] += 1
        most_common_condition = max(condition_counts, key=condition_counts.get)
        
        print("\nTren Cuaca:")
        print(f"- Rata-rata suhu: {avg_temp:.1f}°C")
        print(f"- Rata-rata kelembaban: {avg_humidity:.1f}%")
        print(f"- Kondisi cuaca paling umum: {most_common_condition}")
        
        # Tren perubahan suhu
        first_temp = self.weather_data[sorted_dates[0]]['suhu']
        last_temp = self.weather_data[sorted_dates[-1]]['suhu']
        temp_change = last_temp - first_temp
        
        if temp_change > 0:
            print(f"- Suhu cenderung meningkat (+{temp_change:.1f}°C)")
        elif temp_change < 0:
            print(f"- Suhu cenderung menurun ({temp_change:.1f}°C)")
        else:
            print("- Suhu cenderung stabil")
    
    @staticmethod
    def get_recommendation(condition):
        """Memberikan rekomendasi berdasarkan kondisi cuaca"""
        recommendations = {
            'cerah': "Gunakan tabir surya dan topi",
            'hujan': "Bawa payung atau jas hujan",
            'berawan': "Siapkan jaket ringan"
        }
        return recommendations.get(condition, "Tidak ada rekomendasi khusus")
    
    def run(self):
        """Menjalankan aplikasi utama"""
        while True:
            print("\nAplikasi Pemantauan Cuaca Harian")
            print("1. Masukkan Data Cuaca Hari Ini")
            print("2. Lihat Data Cuaca Beberapa Hari Terakhir")
            print("3. Lihat Tren Cuaca")
            print("4. Keluar")
            
            choice = input("Pilih menu (1-4): ")
            
            if choice == '1':
                self.input_weather_data()
            elif choice == '2':
                self.display_recent_weather()
            elif choice == '3':
                self.weather_trends()
            elif choice == '4':
                print("Terima kasih telah menggunakan aplikasi ini!")
                break
            else:
                print("Pilihan tidak valid. Silakan pilih 1-4.")

if __name__ == "__main__":
    app = WeatherMonitor()
    app.run()
