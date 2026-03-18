---
layout: none
---
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eurasia Paper Magazine</title>
    <link href="https://cdn.jsdelivr.net/gh/spoqa/spoqa-han-sans@latest/css/SpoqaHanSansNeo.css" rel="stylesheet" type="text/css">
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

        /* 헤더 - 페이지 최상단 가운데 */
        .header {
            text-align: center;
            padding: 30px 0 10px 0;
        }

        .logo {
            font-family: 'Herculanum', Georgia, 'Times New Roman', serif;
            font-size: 11pt;
            font-weight: bold;
            line-height: 0.9;
            cursor: pointer;
            transition: opacity 0.3s;
            display: inline-block;
            text-shadow: 0.5px 0 0 black, -0.5px 0 0 black, 0 0.5px 0 black, 0 -0.5px 0 black;
        }

        .logo:hover {
            opacity: 0.6;
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
            grid-template-columns: 1fr 1fr;
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
        }

        .mobile-modal.active {
            display: flex;
        }

        .mobile-modal img {
            max-width: 90%;
            max-height: 90%;
            object-fit: contain;
        }

        a {
            text-decoration: none;
            color: inherit;
        }

        /* 모바일 */
        @media (max-width: 767px) {
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
    <!-- 헤더 - 페이지 최상단 가운데 -->
    <div class="header">
        <a href="/">
            <div class="logo">
                Eurasia<br>
                Paper<br>
                Magazine
            </div>
        </a>
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
        <img src="" alt="" id="mobileModalImage">
    </div>

    <script>
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
                    // 두 번째 클릭: 페이지 이동 (기본 동작)
                }
            });
        });

        // 모달 이미지 클릭 시 페이지 이동
        document.getElementById('mobileModal').addEventListener('click', function() {
            if (currentUrl) {
                window.location.href = currentUrl;
            } else {
                this.classList.remove('active');
                mobileImageShown = false;
                currentSlug = null;
            }
        });
    </script>
</body>
</html>