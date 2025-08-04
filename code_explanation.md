
# 코드 설명

## `components` 폴더

### `Banner.tsx`

```tsx
// `Banner`라는 이름의 함수형 컴포넌트를 정의합니다.
const Banner = () => {
  return (
    // 파란색 배경, 흰색 텍스트, 가운데 정렬, 상하 패딩, 상하 마진을 가진 div 요소를 렌더링합니다.
    <div className="bg-blue-500 text-white text-center py-20 my-8">
      {/* "Welcome to PixelShop!"이라는 텍스트를 가진 h1 요소를 렌더링합니다. */}
      <h1 className="text-5xl font-bold mb-4">Welcome to PixelShop!</h1>
      {/* "Your one-stop shop for exclusive streamer merchandise."라는 텍스트를 가진 p 요소를 렌더링합니다. */}
      <p className="text-xl">Your one-stop shop for exclusive streamer merchandise.</p>
    </div>
  );
};

// `Banner` 컴포넌트를 다른 파일에서 사용할 수 있도록 내보냅니다.
export default Banner;
```

### `Footer.tsx`

```tsx
// `Footer`라는 이름의 함수형 컴포넌트를 정의합니다.
const Footer = () => {
  return (
    // 회색 배경, 회색 텍스트, 작은 텍스트 크기, 상단 마진을 가진 footer 요소를 렌더링합니다.
    <footer className="bg-gray-100 text-gray-600 text-sm mt-12">
      {/* 컨테이너, 좌우 패딩, 상하 패딩을 가진 div 요소를 렌더링합니다. */}
      <div className="container mx-auto px-4 py-8">
        {/* 그리드 레이아웃을 가진 div 요소를 렌더링합니다. */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
          {/* 고객센터 정보를 담는 div 요소를 렌더링합니다. */}
          <div>
            {/* "고객센터"라는 텍스트를 가진 h3 요소를 렌더링합니다. */}
            <h3 className="font-bold text-lg mb-2">고객센터</h3>
            {/* "1588-XXXX"라는 텍스트를 가진 p 요소를 렌더링합니다. */}
            <p className="font-bold text-2xl">1588-XXXX</p>
            {/* 운영시간 정보를 담는 p 요소를 렌더링합니다. */}
            <p>운영시간: 평일 10:00 ~ 18:00</p>
            {/* 점심시간 정보를 담는 p 요소를 렌더링합니다. */}
            <p>점심시간: 12:30 ~ 13:30</p>
            {/* 주말 및 공휴일 휴무 정보를 담는 p 요소를 렌더링합니다. */}
            <p>주말 및 공휴일 휴무</p>
          </div>
          {/* 회사정보를 담는 div 요소를 렌더링합니다. */}
          <div>
            {/* "회사정보"라는 텍스트를 가진 h3 요소를 렌더링합니다. */}
            <h3 className="font-bold text-lg mb-2">회사정보</h3>
            {/* 회사명 정보를 담는 p 요소를 렌더링합니다. */}
            <p>회사명: 주식회사 픽셀샵</p>
            {/* 대표 정보를 담는 p 요소를 렌더링합니다. */}
            <p>대표: 홍길동</p>
            {/* 주소 정보를 담는 p 요소를 렌더링합니다. */}
            <p>주소: 서울특별시 강남구 테헤란로 123</p>
            {/* 사업자등록번호 정보를 담는 p 요소를 렌더링합니다. */}
            <p>사업자등록번호: 123-45-67890</p>
          </div>
          {/* 이용안내 정보를 담는 div 요소를 렌더링합니다. */}
          <div>
            {/* "이용안내"라는 텍스트를 가진 h3 요소를 렌더링합니다. */}
            <h3 className="font-bold text-lg mb-2">이용안내</h3>
            {/* 링크 목록을 담는 ul 요소를 렌더링합니다. */}
            <ul>
              {/* "회사소개" 링크를 렌더링합니다. */}
              <li><a href="#" className="hover:text-gray-900">회사소개</a></li>
              {/* "이용약관" 링크를 렌더링합니다. */}
              <li><a href="#" className="hover:text-gray-900">이용약관</a></li>
              {/* "개인정보처리방침" 링크를 렌더링합니다. */}
              <li><a href="#" className="hover:text-gray-900">개인정보처리방침</a></li>
              {/* "고객센터" 링크를 렌더링합니다. */}
              <li><a href="#" className="hover:text-gray-900">고객센터</a></li>
            </ul>
          </div>
        </div>
        {/* 저작권 정보를 담는 div 요소를 렌더링합니다. */}
        <div className="border-t mt-8 pt-6 text-center">
          {/* 저작권 텍스트를 렌더링합니다. */}
          <p>COPYRIGHT ⓒ 2024 주식회사 픽셀샵. ALL RIGHTS RESERVED.</p>
        </div>
      </div>
    </footer>
  );
};

// `Footer` 컴포넌트를 다른 파일에서 사용할 수 있도록 내보냅니다.
export default Footer;
```

### `Header.tsx`

```tsx
// `Header`라는 이름의 함수형 컴포넌트를 정의합니다.
const Header = () => {
  return (
    // 흰색 배경, 그림자 효과를 가진 header 요소를 렌더링합니다.
    <header className="bg-white shadow-md">
      {/* 컨테이너, 좌우 패딩을 가진 div 요소를 렌더링합니다. */}
      <div className="container mx-auto px-4">
        {/* Top bar: 오른쪽 정렬, 수직 가운데 정렬, 상하 패딩, 작은 텍스트, 회색 텍스트, 하단 테두리를 가진 div 요소를 렌더링합니다. */}
        <div className="flex justify-end items-center py-2 text-sm text-gray-500 border-b">
          {/* "로그인" 링크를 렌더링합니다. */}
          <a href="#" className="ml-4 hover:text-gray-800">로그인</a>
          {/* "마이페이지" 링크를 렌더링합니다. */}
          <a href="#" className="ml-4 hover:text-gray-800">마이페이지</a>
          {/* "장바구니" 링크를 렌더링합니다. */}
          <a href="#" className="ml-4 hover:text-gray-800">장바구니</a>
          {/* "고객센터" 링크를 렌더링합니다. */}
          <a href="#" className="ml-4 hover:text-gray-800">고객센터</a>
        </div>

        {/* Main header: 좌우 정렬, 수직 가운데 정렬, 상하 패딩을 가진 div 요소를 렌더링합니다. */}
        <div className="flex justify-between items-center py-6">
          {/* "PixelShop" 로고를 렌더링합니다. */}
          <div className="text-3xl font-extrabold text-blue-600">
            <a href="/">PixelShop</a>
          </div>
          {/* 검색창을 담는 div 요소를 렌더링합니다. */}
          <div className="relative w-1/3">
            {/* 검색어 입력창을 렌더링합니다. */}
            <input
              type="text"
              placeholder="검색어를 입력하세요"
              className="w-full py-2 px-4 border border-gray-300 rounded-full focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            {/* 검색 버튼을 렌더링합니다. */}
            <button className="absolute right-0 top-0 mt-2 mr-3">
              {/* 검색 아이콘을 렌더링합니다. */}
              <svg className="w-5 h-5 text-gray-400" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fillRule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clipRule="evenodd"></path></svg>
            </button>
          </div>
          {/* 추후 아이콘이 추가될 공간입니다. */}
          <div>
            {/* Future icons */}
          </div>
        </div>
      </div>

      {/* Navigation: 흰색 배경, 상하 테두리를 가진 nav 요소를 렌더링합니다. */}
      <nav className="bg-white border-t border-b">
        {/* 컨테이너, 가운데 정렬, 수직 가운데 정렬, 상하 패딩을 가진 div 요소를 렌더링합니다. */}
        <div className="container mx-auto flex justify-center items-center py-3">
          {/* "NEW" 링크를 렌더링합니다. */}
          <a href="#" className="mx-6 text-gray-800 hover:text-blue-600 font-bold">NEW</a>
          {/* "STREAMER" 링크를 렌더링합니다. */}
          <a href="#" className="mx-6 text-gray-800 hover:text-blue-600 font-bold">STREAMER</a>
          {/* "FASHION" 링크를 렌더링합니다. */}
          <a href="#" className="mx-6 text-gray-800 hover:text-blue-600 font-bold">FASHION</a>
          {/* "DOLL" 링크를 렌더링합니다. */}
          <a href="#" className="mx-6 text-gray-800 hover:text-blue-600 font-bold">DOLL</a>
          {/* "KEYRING" 링크를 렌더링합니다. */}
          <a href="#" className="mx-6 text-gray-800 hover:text-blue-600 font-bold">KEYRING</a>
          {/* "SALE" 링크를 렌더링합니다. */}
          <a href="#" className="mx-6 text-gray-800 hover:text-blue-600 font-bold">SALE</a>
        </div>
      </nav>
    </header>
  );
};

// `Header` 컴포넌트를 다른 파일에서 사용할 수 있도록 내보냅니다.
export default Header;
```

### `ProductCard.tsx`

```tsx
// `Product`라는 이름의 인터페이스를 정의합니다.
interface Product {
  id: number;
  name: string;
  price: number;
  image: string;
}

// `ProductCard`라는 이름의 함수형 컴포넌트를 정의합니다. `product` 객체를 props로 받습니다.
const ProductCard = ({ product }: { product: Product }) => {
  return (
    // 흰색 배경, 둥근 모서리, 그림자 효과, 호버 시 그림자 효과 변경, 트랜지션, 커서 포인터, 넘치는 부분 숨김을 가진 div 요소를 렌더링합니다.
    <div className="bg-white rounded-xl shadow-md hover:shadow-xl transition-shadow duration-300 cursor-pointer overflow-hidden">
      {/* 이미지 비율을 유지하는 div 요소를 렌더링합니다. */}
      <div className="aspect-w-1 aspect-h-1">
        {/* 상품 이미지를 렌더링합니다. */}
        <img
          src={product.image}
          alt={product.name}
          className="object-cover w-full h-full"
        />
      </div>
      {/* 상품 정보를 담는 div 요소를 렌더링합니다. */}
      <div className="p-5">
        {/* 상품 이름을 렌더링합니다. */}
        <h3 className="text-lg font-semibold mb-2">{product.name}</h3>
        {/* 상품 가격을 렌더링합니다. */}
        <p className="text-indigo-600 font-bold text-xl">
          {product.price.toLocaleString()}원
        </p>
        {/* 버튼들을 담는 div 요소를 렌더링합니다. */}
        <div className="mt-4 flex space-x-2">
          {/* "옵션 보기" 버튼을 렌더링합니다. */}
          <button className="w-1/2 bg-gray-200 text-gray-800 py-2 rounded-lg hover:bg-gray-300 transition-colors">
            옵션 보기
          </button>
          {/* "장바구니" 버튼을 렌더링합니다. */}
          <button className="w-1/2 bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition-colors">
            장바구니
          </button>
        </div>
      </div>
    </div>
  );
};

// `ProductCard` 컴포넌트를 다른 파일에서 사용할 수 있도록 내보냅니다.
export default ProductCard;
```

## `pages` 폴더

### `Home.tsx`

```tsx
// `Header`, `Banner`, `ProductCard`, `Footer` 컴포넌트를 가져옵니다.
import Header from '../components/Header';
import Banner from '../components/Banner';
import ProductCard from '../components/ProductCard';
import Footer from '../components/Footer';

// 임시 상품 데이터를 정의합니다.
const mockProducts = [
  {
    id: 1,
    name: '[PRE-ORDER] 스트리머 A 티셔츠',
    price: 29000,
    image: 'https://via.placeholder.com/300x300.png?text=Product+A',
  },
  // ... (다른 상품 데이터)
];

// `Home`이라는 이름의 함수형 컴포넌트를 정의합니다.
const Home = () => {
  return (
    // 회색 배경을 가진 div 요소를 렌더링합니다.
    <div className="bg-gray-50">
      {/* `Header` 컴포넌트를 렌더링합니다. */}
      <Header />
      {/* 메인 콘텐츠 영역을 렌더링합니다. */}
      <main className="container mx-auto px-4">
        {/* `Banner` 컴포넌트를 렌더링합니다. */}
        <Banner />
        
        {/* "NEW ARRIVALS" 섹션을 렌더링합니다. */}
        <section>
          {/* "NEW ARRIVALS"라는 텍스트를 가진 h2 요소를 렌더링합니다. */}
          <h2 className="text-3xl font-bold text-center my-12">NEW ARRIVALS</h2>
          {/* 상품 목록을 그리드 레이아웃으로 렌더링합니다. */}
          <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8">
            {/* `mockProducts` 배열을 순회하며 각 상품에 대한 `ProductCard` 컴포넌트를 렌더링합니다. */}
            {mockProducts.map(product => (
              <ProductCard key={product.id} product={product} />
            ))}
          </div>
        </section>

      </main>
      {/* `Footer` 컴포넌트를 렌더링합니다. */}
      <Footer />
    </div>
  );
};

// `Home` 컴포넌트를 다른 파일에서 사용할 수 있도록 내보냅니다.
export default Home;
```
