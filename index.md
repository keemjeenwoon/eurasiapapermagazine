---
layout: none
---
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eurasia Paper Magazine</title>
    <link href="https://cdn.jsdelivr.net/gh/spoqa/spoqa-han-sans@latest/css/SpoqaHanSansNeo.css" rel="stylesheet" type="text/css">
    <link href="https://fonts.googleapis.com/css2?family=Aboreto&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: #ffffff;
            font-family: 'Spoqa Han Sans Neo', -apple-system, sans-serif;
            color: #000;
            overflow-x: hidden;
        }

        /* 헤더 - 전광판 애니메이션 */
        .header {
            display: flex;
            justify-content: flex-start;
            padding: 30px 12.5% 20px 12.5%;
        }

        .logo-container {
            width: 100%;
            overflow: hidden;
        }

        .logo {
            font-family: 'Herculanum', Georgia, 'Times New Roman', serif;
            font-size: 24pt;
            font-weight: bold;
            white-space: nowrap;
            text-shadow: 0.5px 0 0 black, -0.5px 0 0 black, 0 0.5px 0 black, 0 -0.5px 0 black;
        }

        .logo-scroll {
            display: inline-block;
            animation: scroll 15s linear infinite;
        }

        @keyframes scroll {
            0% { transform: translateX(0); }
            100% { transform: translateX(-50%); }
        }

        /* 메인 컨테이너 */
        .main-container {
            display: flex;
            justify-content: center;
            padding: 0 12.5%;
        }

        .wrapper {
            width: 100%;
            display: grid;
            grid-template-columns: 3fr 4fr;
            gap: 0;
        }

        /* 왼쪽: 글 목록 */
        .list {
            padding: 0;
            text-align: right;
        }

        .post-item {
            margin-bottom: 60px;
            cursor: pointer;
            transition: opacity 0.3s;
        }

        .post-item:hover {
            opacity: 0.6;
        }

        .post-category {
            font-family: 'Courier New', 'Courier', monospace;
            font-size: 14pt;
            font-weight: normal;
            letter-spacing: 0.5px;
            display: inline-block;
            padding-right: 2px;
        }

        /* 오른쪽: 이미지 표시 영역 */
        .preview {
            position: relative;
            background: #ffffff;
            min-height: 600px;
        }

        .preview-image {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .preview-image.active {
            opacity: 1;
        }

        /* 모바일 이미지 모달 */
        .mobile-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.95);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .mobile-modal.active {
            display: flex;
        }

        .mobile-modal-image-wrapper {
            max-width: 90%;
            max-height: 90%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .mobile-modal img {
            max-width: 100%;
            max-height: 100%;
            object-fit: contain;
        }

        a {
            text-decoration: none;
            color: inherit;
        }

        /* 모바일 */
        @media (max-width: 767px) {
            .header {
                padding: 30px 5% 20px 5%;
                justify-content: center;
            }

            .logo-container {
                width: 90%;
            }

            .logo {
                font-family: 'Aboreto', sans-serif;
                font-weight: bold;
                font-size: 20pt;
                letter-spacing: -1px;
            }

            .main-container {
                padding: 0 5%;
            }

            .wrapper {
                grid-template-columns: 1fr;
            }

            .list {
                padding: 20px 0 40px 0;
                text-align: left;
            }

            .preview {
                display: none;
            }
        }
    </style>
</head>
<body>
    <!-- 헤더 - 전광판 애니메이션 -->
    <div class="header">
        <div class="logo-container">
            <div class="logo">
                <span class="logo-scroll">
                    Eurasia Paper Magazine&nbsp;&nbsp;&nbsp;&nbsp;Eurasia Paper Magazine&nbsp;&nbsp;&nbsp;&nbsp;Eurasia Paper Magazine&nbsp;&nbsp;&nbsp;&nbsp;Eurasia Paper Magazine&nbsp;&nbsp;&nbsp;&nbsp;
                </span>
            </div>
        </div>
    </div>

    <!-- 메인 컨텐츠 -->
    <div class="main-container">
        <div class="wrapper">
            <!-- 왼쪽: 글 목록 -->
            <div class="list">
                <a href="/2026/03/17/Kasseta.html" class="post-link" data-url="/2026/03/17/Kasseta.html">
                    <div class="post-item" 
                         data-slug="kasseta"
                         onmouseenter="showImage(this)"
                         onmouseleave="hideImage()">
                        <div class="post-category">[Interview][POCC][КАССЕТА]</div>
                    </div>
                </a>
            </div>

            <!-- 오른쪽: 이미지 표시 -->
            <div class="preview" id="preview">
                <img src="/assets/images/covers/kassetacover.png" 
                     alt="КАССЕТА" 
                     class="preview-image" 
                     data-slug="kasseta">
            </div>
        </div>
    </div>

    <!-- 모바일 이미지 모달 -->
    <div class="mobile-modal" id="mobileModal">
        <div class="mobile-modal-image-wrapper">
            <img src="" alt="" id="mobileModalImage">
        </div>
    </div>

    <script>
        // 헤더 트랙 너비 동적 조정
        function adjustHeaderTrack() {
            if (window.innerWidth > 767) {
                const postCategory = document.querySelector('.post-category');
                const listColumn = document.querySelector('.list');
                const wrapper = document.querySelector('.wrapper');
                const header = document.querySelector('.header');
                const logoContainer = document.querySelector('.logo-container');
                
                if (postCategory && listColumn && wrapper && header && logoContainer) {
                    // 글자 너비 측정
                    const textWidth = postCategory.offsetWidth;
                    
                    // 글목록 컬럼 너비
                    const listWidth = listColumn.offsetWidth;
                    
                    // 전체 wrapper 너비
                    const wrapperWidth = wrapper.offsetWidth;
                    
                    // 왼쪽 여백 = 글목록 너비 - 글자 너비
                    const leftGap = listWidth - textWidth;
                    
                    // 트랙 시작 위치 = 페이지 여백(12.5%) + 왼쪽 여백
                    const viewportWidth = window.innerWidth;
                    const pageMargin = viewportWidth * 0.125;
                    const trackStart = pageMargin + leftGap;
                    
                    // 트랙 너비 = 전체 - 왼쪽 여백
                    const trackWidth = wrapperWidth - leftGap;
                    
                    // 헤더 padding 조정
                    header.style.paddingLeft = trackStart + 'px';
                    header.style.paddingRight = pageMargin + 'px';
                    
                    // 트랙 컨테이너 너비 조정
                    logoContainer.style.width = trackWidth + 'px';
                }
            }
        }
        
        // 페이지 로드 시 실행
        window.addEventListener('load', adjustHeaderTrack);
        
        // 윈도우 리사이즈 시 재조정
        window.addEventListener('resize', adjustHeaderTrack);

        let mobileImageShown = false;
        let currentSlug = null;
        let currentUrl = null;

        function showImage(element) {
            const slug = element.getAttribute('data-slug');
            
            // 모든 이미지 숨기기
            document.querySelectorAll('.preview-image').forEach(img => {
                img.classList.remove('active');
            });
            
            // 해당 이미지만 표시
            const targetImage = document.querySelector(`.preview-image[data-slug="${slug}"]`);
            if (targetImage) {
                targetImage.classList.add('active');
            }
        }

        function hideImage() {
            document.querySelectorAll('.preview-image').forEach(img => {
                img.classList.remove('active');
            });
        }

        // 모바일 터치 이벤트
        document.querySelectorAll('.post-link').forEach(link => {
            link.addEventListener('click', function(e) {
                if (window.innerWidth <= 767) {
                    const slug = this.querySelector('.post-item').getAttribute('data-slug');
                    const url = this.getAttribute('data-url');
                    
                    if (!mobileImageShown || currentSlug !== slug) {
                        e.preventDefault();
                        
                        // 이미지 모달 표시
                        const modal = document.getElementById('mobileModal');
                        const modalImage = document.getElementById('mobileModalImage');
                        modalImage.src = '/assets/images/covers/kassetacover.png';
                        modal.classList.add('active');
                        
                        mobileImageShown = true;
                        currentSlug = slug;
                        currentUrl = url;
                    }
                }
            });
        });

        // 모달 배경 클릭 시 닫기
        document.getElementById('mobileModal').addEventListener('click', function(e) {
            if (e.target === this) {
                this.classList.remove('active');
                mobileImageShown = false;
                currentSlug = null;
                currentUrl = null;
            }
        });

        // 이미지 클릭 시 페이지 이동
        document.getElementById('mobileModalImage').addEventListener('click', function(e) {
            e.stopPropagation();
            if (currentUrl) {
                window.location.href = currentUrl;
            }
        });
    </script>
</body>
</html>