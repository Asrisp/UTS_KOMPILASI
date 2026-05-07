1. Mengapa fungsi power() harus dipanggil di dalam term(), bukan sebaliknya? Jelaskan kaitannya dengan Operator Precedence.
Jawab:

Dalam Operator Precedence operator ^ memiliki prioritas lebih tinggi dibanding * dan /. Aturan ini diterapkan menggunakan metode Recursive Descent Parsing di mana setiap fungsi parser merepresentasikan tingkat prioritas operator tertentu.
Hierarkinya sbb: 
expr() menangani operator + dan -
     term() menangani operator * dan /
          power() menangani operator ^
               factor() menangani angka, variabel, dan tanda kurung

pada fungsi power() ada 
if self._current == '^': 
op = self._current
dimana jika ada simbol '^' maka jalankan terlebih dahulu.

kemudian pada fungsi term() 
node = self.power() 
while self._current in ('*', '/'):
term() memanggil power() terlebih dahulu agar menyelesaikan dulu semua yang ditangani power() setelah itu baru ke fungsi term sendiri.

Hal ini juga sesuai dengan konsep hierarki matematika dimana pangkat memiliki prioritas lebih tinggi daripada perkalian (*) dan pembagian (/).


2. Apa yang terjadi pada fase Analisis Semantik jika variabel z digunakan dalam kode sumber tetapi tidak ada di symbol_table?
elif token and token.isalpha():
  if token not in self._env:
    raise ParserError(f"Semantic Error: Undefined variable '{token}'")

pada fungsi factor() terdapat pengecekan jika token adalah variabel, tetapi tidak ada di self._env, maka compiler akan menghasilkan error semantik.

source_code = "a ^ 2 + b * c" dengan symbol_table = {'a': 5, 'b': 10, 'c': 2}
tapi jika sourcenya diubah misal "z ^ 2 + b * c"
maka compiler akan menginformasikan error	


3. Jelaskan mengapa dalam TAC, instruksi untuk a ^ 2 harus muncul sebelum instruksi untuk +.
Pada generate_tac()
left_val  = self.generate_tac(node.left)
right_val = self.generate_tac(node.right)
artinya: baca instruksi kiri dulu ke kanan 
source_code = "a ^ 2 + b * c" 
maka TAC yang dihasilkan terlebih dahulu adalah: t1 = a^2, sementara karena '+' merupakan parent menunggu subtree kiri dan kanan selesai diproses jadi dilanjutkan ke b*c sebagai t2, lalu terakhir t1 + t2



