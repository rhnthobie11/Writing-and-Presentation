# Writing Week-3 FE Bootcamp

## React Context
- Context membantu kita untuk bsia mengakses dimana saja tanpa harus melakukan parsing props ke setiap komponen. (seperti variable global)
- Context direkomendasikan dalam penggunaan aplikasi skala kecil ke menengah saja
- berguna agar mudah dikembangkan dan dimengerti oleh orang-orang

pembuatan Context
```js
import { createContext } from 'react'

export default createContext()
```

- mengubah state dengan `dispatch()` yang disediakan `useReducer()` dari react
- `dispatch()` menerima object, type dan payload.

hal yang harus diingat ketika membuat context!
1. State
2. Reducer
3. Type
4. CreateContext()

**kesimpulan**
>penggunaan Context membuat code mu menjadi lebih mudah dibaca, mudah diatur, dan menjadi **terstruktur**
<hr>

## React Testing
- React Testing Library adalah seperangkat **helpers* yang memungkinkan Anda mengetes komponen pada React tanpa bergantung pada detail implementasinya. Pendekatan ini membuat refactoring menjadi mudah dan juga mendorong Anda untuk menerapkan best practices untuk aksesbilitas
- Jest adalah test runner pada JavaScript yang memungkinkan Anda mengakses DOM melalui jsdom. Sementara jsdom hanyalah perkiraan cara kerja browser, seringkali cukup baik mengetes komponen pada React. Jest memberikan kecepatan iterasi yang bagus dikombinasikan dengan fitur-fitur yang powerful seperti mocking modules dan timers sehingga Anda dapat memiliki kontrol lebih pada kode yang dijalankan.

### Unit Testing
- Unit testing merupakan salah satu metode testing terhadap fungsi ataupun usecase yang ada pada aplikasi.
- dengan dilakukannya unit test ini, akan mudah menentukan apakah fungsi tersebut sudah sesuai dengan apa yang diharapkan.\
- mempermudah kita dalam melakukan debugging.

### keuntungan unit testing
a. Waktu yang relatif lebih singkat dalam melakukan pengujian.

b. Tidak perlu tergantung pada koneksi maupun keadaan database, berbeda dengan integration test yang sangat tergantung pada koneksi dan keadaan data pada database.

c. Lebih mudah dalam mencari bug ataupun error pada kode

d. Sangat berguna dalam dokumentasi software

e. Meningkatkan kemampuan re-usable kode

### Membuat Fungsi Mock Dengan Menggunakan Jest
- membuat tiruan/mock dari suatu fungsi dan membuat balikannya secara manual, sehingga tidak peduli apakah fungsi tersebut bekerja, fungsi akan mengembalikan nilai sesuai yang kita definisikan.

```js
const fungsiTambah = jest.fn((x,y)=>3);
test(‘Calculator.jsx Unit Test’, () => {
expect(fungsiTambah(1,2)).toEqual(3);
})
```

- Pada kode diatas kita membuat sebuah fake function dengan nama fungsiTambah dan kita memberikan nilai balikan langsung pada fungsi tersebut. Mungkin dari kita bingung apa gunanya sebuah mock atau fake function ini. 
- Fake function ini sangat berguna pada saat kita akan melakukan testing terhadap fungsi yang memiliki suatu koneksi ke database. Agar kita tidak perlu terkoneksi ke database, kita hanya perlu membuat suatu mock function tersebut, sehingga hasil pengujian kita akan lebih independen karena tidak perlu terpengaruh pada faktor eksternal. Kita akan coba mengenal beberapa fungsi lagi yang berguna sebagai verifikasi.

```js
test(‘Calculator.jsx Unit Test’, (x,y) => {
expect(fungsiTambah(1,2)).toEqual(3);
expect(fungsiTambah).toHaveBeenCalledTimes(1);
expect(fungsiTambah).toHaveBeenCalledWith(1,4);
})
```

- Fungsi verifikasi yang kita tambahkan adalah fungsi toHaveBeenCalledTimes() dan toHaveBeenCalledWith(). 
- Fungsi toHaveBeenCalledTimes() berfungsi untuk mengecek apakah suatu fungsi pernah dipanggil sebanyak jumlah yang kita definisikan.

Jest menggunakan matchers untuk membandingkan nilai. Anda dapat menggunakannya untuk memeriksa kesetaraan, membandingkan dua nomor atau string, dan memverifikasi truthiness ekspresi. Berikut ini adalah daftar matchers populer yang tersedia dalam Jest.

- toBe();
- toBeNull()
- toBeDefined()
- toBeUndefined()
- toBeTruthy()
- toBeFalsy()
- toBeGreaterThan()
- toBeLesserThan()
- toMatch()
- toContain()

untuk lebih lengkapnya bisa kunjungi [disini](https://jestjs.io/docs/using-matchers)

### Menguji komponen React
- Pertama, kita akan menulis beberapa tes untuk komponen `ProductHeader`.

src/components/ProductHeader.js
```js
import React, {Component} from 'react';
    
class ProductHeader extends Component  {
    render() {
        return(
            <h2 className="title"> Product Listing Page </h2>
        );
    }
};
export default ProductHeader;
```

Kita bisa menulis tes dengan asumsi berikut:

1. Komponen harus merender tag h2.
2. Tag h2 harus memiliki kelas bernama title.

Untuk membuat komponen dan untuk mengambil node DOM yang relevan, kita perlu ReactTestUtils. Hapus spesifikasi dummy dan tambahkan kode berikut:

src/Components/__tests__/ProductHeader.Test.js
```js
import React from 'react';
import ReactTestUtils from 'react-dom/test-utils'; 
import ProductsList from '../ProductsList';
 
describe('ProductHeader Component', () => {
 
    it('has an h2 tag', () => {
     //Test here
    });
   
    it('is wrapped inside a title class', () => {
     //Test here
    })
  })
```

Untuk memeriksa keberadaan node `h2`, pertama-tama harus membuat elemen React menjadi sebuah node DOM di dokumen. Anda dapat melakukannya dengan bantuan beberapa API yang diekspor oleh `ReactTestUtils`. Misalnya, untuk membuat komponen `<ProductHeader>` kita, Anda dapat melakukan sesuatu seperti ini:
```js
const component = ReactTestUtils.renderIntoDocument(<ProductHeader/>);
```

Kemudian, kita dapat mengekstrak tag `h2` dari komponen dengan bantuan `findRenderedDOMComponentWithTag ('tag-name')`. Memeriksa semua node anak dan menemukan node yang sesuai tag-name.

```js
  it('has an h2 tag', () => {
 
    const component = ReactTestUtils.renderIntoDocument(<ProductHeader/>);    
    var h2 = ReactTestUtils.findRenderedDOMComponentWithTag(
     component, 'h2'
   );
   
});
```

Sekarang, coba buat kode untuk tes kedua. Anda dapat menggunakan `findRenderedDOMcomponentWithClass()` untuk memeriksa apakah ada setiap node dengan kelas 'title'.
```js
it('has a title class', () => {
 
  const component = ReactTestUtils.renderIntoDocument(<ProductHeader/>);    
  var node = ReactTestUtils.findRenderedDOMComponentWithClass(
   component, 'title'
 );
})
```

Itu dia! Jika semua berjalan dengan baik, Anda akan melihat hasilnya dalam warna hijau.

![img](https://cms-assets.tutsplus.com/cdn-cgi/image/width=630/uploads/users/1795/posts/28934/image/Testing-Components-in-React-PassingSpec.png)



