---
layout: none
---
<!DOCTYPE html>
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

        .container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            min-height: 100vh;
        }

        /* 왼쪽: 글 목록 */
        .list {
            padding: 60px 40px;
            border-right: 1px solid #ddd;
        }

        .post-item {
            margin-bottom: 40px;
            cursor: pointer;
            transition: opacity 0.3s;
        }

        .post-item:hover {
            opacity: 0.6;
        }

        .post-title {
            font-family: 'Times New Roman', Times, serif;
            font-size: 16pt;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .post-meta {
            font-family: 'Times New Roman', Times, serif;
            font-size: 8pt;
            text-transform: uppercase;
            color: #666;
        }

        /* 오른쪽: 이미지 표시 영역 */
        .preview {
            position: relative;
            background: #f5f5f5;
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

        /* 모바일 */
        @media (max-width: 767px) {
            .container {
                grid-template-columns: 1fr;
            }

            .list {
                border-right: none;
                padding: 30px 20px;
            }

            .preview {
                display: none;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 왼쪽: 글 목록 -->
        <div class="list">
            {% for post in site.posts %}
            <div class="post-item" 
                 data-image="/assets/images/covers/{{ post.slug }}.png"
                 onmouseenter="showImage(this)"
                 onmouseleave="hideImage()">
                <div class="post-title">{{ post.title }}</div>
                <div class="post-meta">{{ post.date | date: "%d %b %Y" }}</div>
            </div>
            {% endfor %}
        </div>

        <!-- 오른쪽: 이미지 표시 -->
        <div class="preview" id="preview">
            {% for post in site.posts %}
            <img src="/assets/images/covers/{{ post.slug }}.png" 
                 alt="{{ post.title }}" 
                 class="preview-image" 
                 data-slug="{{ post.slug }}">
            {% endfor %}
        </div>
    </div>

    <script>
        function showImage(element) {
            const imageUrl = element.getAttribute('data-image');
            const slug = imageUrl.split('/').pop().replace('.png', '');
            
            // 모든 이미지 숨기기
            document.querySelectorAll('.preview-image').forEach(img => {
                img.classList.remove('active');
            });
            
            // 해당 이미지만 표시
            const targetImage = document.querySelector(`[data-slug="${slug}"]`);
            if (targetImage) {
                targetImage.classList.add('active');
            }
        }

        function hideImage() {
            document.querySelectorAll('.preview-image').forEach(img => {
                img.classList.remove('active');
            });
        }
    </script>
</body>
</html>