
import io
import colorsys
import numpy as np
import pandas as pd
import streamlit as st

from PIL import Image, ImageOps
from collections import Counter
from math import sqrt


# -------------------- НАСТРОЙКИ --------------------

st.set_page_config(
    page_title="AI Nail Stylist",
    page_icon="💅",
    layout="wide",
    initial_sidebar_state="expanded",
)

st.markdown("""
<style>
    .main {
        background-color: #fff9fc;
    }
    h1, h2, h3 {
        color: #8d4269;
    }
    .nail-card {
        background: #ffffff;
        border: 1px solid #f0dce7;
        border-radius: 16px;
        padding: 16px;
        margin-bottom: 12px;
    }
    .small-muted {
        color: #806b78;
        font-size: 14px;
    }
    div.stButton > button {
        border-radius: 12px;
    }
</style>
""", unsafe_allow_html=True)


# -------------------- КАТАЛОГ ДИЗАЙНОВ --------------------

DESIGNS = [
    {
        "id": 1, "name": "Нежный нюд",
        "style": "Нюд", "colors": ["#E8B9AD", "#D69B91"],
        "occasion": ["На каждый день", "Офис", "Свидание"],
        "length": ["Короткие", "Средние", "Длинные"],
        "shapes": ["Овал", "Миндаль", "Квадрат", "Мягкий квадрат"],
        "season": ["Весна", "Лето", "Осень", "Зима"],
        "difficulty": "Легкий", "price": "Низкая",
        "description": "Полупрозрачный розово-бежевый оттенок с глянцевым финишем.",
        "tags": ["элегантный", "универсальный", "минимализм"],
    },
    {
        "id": 2, "name": "Классический френч",
        "style": "Френч", "colors": ["#F4E6D8", "#FFFFFF"],
        "occasion": ["На каждый день", "Офис", "Свадьба"],
        "length": ["Короткие", "Средние", "Длинные"],
        "shapes": ["Овал", "Миндаль", "Квадрат", "Мягкий квадрат"],
        "season": ["Весна", "Лето", "Осень", "Зима"],
        "difficulty": "Средний", "price": "Средняя",
        "description": "Молочная база и аккуратная белая линия улыбки.",
        "tags": ["классика", "элегантный", "свадебный"],
    },
    {
        "id": 3, "name": "Молочный минимализм",
        "style": "Минимализм", "colors": ["#F5EDE5", "#E9D8D2"],
        "occasion": ["На каждый день", "Офис", "Свадьба"],
        "length": ["Короткие", "Средние"],
        "shapes": ["Овал", "Миндаль", "Квадрат", "Мягкий квадрат"],
        "season": ["Весна", "Лето", "Осень", "Зима"],
        "difficulty": "Легкий", "price": "Низкая",
        "description": "Чистый молочный оттенок, который визуально выглядит аккуратно.",
        "tags": ["минимализм", "чистый", "универсальный"],
    },
    {
        "id": 4, "name": "Романтичный розовый",
        "style": "Романтичный", "colors": ["#E99AB5", "#F4C5D5"],
        "occasion": ["Свидание", "На каждый день", "Свадьба"],
        "length": ["Короткие", "Средние", "Длинные"],
        "shapes": ["Овал", "Миндаль", "Мягкий квадрат"],
        "season": ["Весна", "Лето"],
        "difficulty": "Легкий", "price": "Низкая",
        "description": "Розовая палитра с нежным глянцевым покрытием.",
        "tags": ["женственный", "романтичный", "нежный"],
    },
    {
        "id": 5, "name": "Бордовый шик",
        "style": "Классика", "colors": ["#711F38", "#4A1428"],
        "occasion": ["Вечеринка", "Свидание", "Офис"],
        "length": ["Короткие", "Средние", "Длинные"],
        "shapes": ["Овал", "Миндаль", "Квадрат"],
        "season": ["Осень", "Зима"],
        "difficulty": "Легкий", "price": "Низкая",
        "description": "Глубокий винный оттенок для выразительного образа.",
        "tags": ["вечерний", "элегантный", "насыщенный"],
    },
    {
        "id": 6, "name": "Зеркальный хром",
        "style": "Хром", "colors": ["#C6C5CB", "#E9DCE8"],
        "occasion": ["Вечеринка", "Свидание"],
        "length": ["Средние", "Длинные"],
        "shapes": ["Миндаль", "Квадрат", "Мягкий квадрат"],
        "season": ["Весна", "Лето", "Осень", "Зима"],
        "difficulty": "Сложный", "price": "Высокая",
        "description": "Перламутровое зеркальное сияние с металлическим эффектом.",
        "tags": ["тренд", "сияющий", "эффектный"],
    },
    {
        "id": 7, "name": "Кошачий глаз",
        "style": "Кошачий глаз", "colors": ["#673E78", "#A18BBE"],
        "occasion": ["Вечеринка", "Свидание"],
        "length": ["Средние", "Длинные"],
        "shapes": ["Миндаль", "Овал", "Квадрат"],
        "season": ["Осень", "Зима"],
        "difficulty": "Сложный", "price": "Высокая",
        "description": "Магнитное мерцание с глубиной и переливами.",
        "tags": ["сияющий", "вечерний", "тренд"],
    },
    {
        "id": 8, "name": "Солнечный пастельный",
        "style": "Пастель", "colors": ["#F2D67A", "#A9CFA9"],
        "occasion": ["На каждый день", "Отпуск", "Вечеринка"],
        "length": ["Короткие", "Средние"],
        "shapes": ["Овал", "Квадрат", "Мягкий квадрат"],
        "season": ["Весна", "Лето"],
        "difficulty": "Средний", "price": "Средняя",
        "description": "Светлые пастельные оттенки, вдохновленные летней палитрой.",
        "tags": ["яркий", "свежий", "летний"],
    },
    {
        "id": 9, "name": "Красная классика",
        "style": "Классика", "colors": ["#C6283D", "#9F172D"],
        "occasion": ["Вечеринка", "Свидание", "На каждый день"],
        "length": ["Короткие", "Средние", "Длинные"],
        "shapes": ["Овал", "Миндаль", "Квадрат", "Мягкий квадрат"],
        "season": ["Весна", "Лето", "Осень", "Зима"],
        "difficulty": "Легкий", "price": "Низкая",
        "description": "Яркий красный маникюр — выразительный и вне времени.",
        "tags": ["классика", "яркий", "смелый"],
    },
    {
        "id": 10, "name": "Цветочная нежность",
        "style": "Цветочный", "colors": ["#E8A7BB", "#A8C69F"],
        "occasion": ["Свидание", "На каждый день", "Свадьба"],
        "length": ["Средние", "Длинные"],
        "shapes": ["Овал", "Миндаль", "Мягкий квадрат"],
        "season": ["Весна", "Лето"],
        "difficulty": "Сложный", "price": "Высокая",
        "description": "Миниатюрные цветы на нежной базе.",
        "tags": ["романтичный", "цветочный", "нежный"],
    },
    {
        "id": 11, "name": "Шоколадный латте",
        "style": "Нюд", "colors": ["#95664F", "#C49A7C"],
        "occasion": ["На каждый день", "Офис", "Свидание"],
        "length": ["Короткие", "Средние", "Длинные"],
        "shapes": ["Овал", "Миндаль", "Квадрат"],
        "season": ["Осень", "Зима"],
        "difficulty": "Легкий", "price": "Низкая",
        "description": "Теплая кофейная палитра — от карамели до шоколада.",
        "tags": ["спокойный", "натуральный", "элегантный"],
    },
    {
        "id": 12, "name": "Праздничный блеск",
        "style": "Блестки", "colors": ["#D6B5D9", "#E6C77A"],
        "occasion": ["Вечеринка", "Свадьба", "Отпуск"],
        "length": ["Средние", "Длинные"],
        "shapes": ["Миндаль", "Квадрат", "Мягкий квадрат"],
        "season": ["Весна", "Лето", "Осень", "Зима"],
        "difficulty": "Средний", "price": "Средняя",
        "description": "Сияющие акценты для праздничного настроения.",
        "tags": ["праздничный", "сияющий", "эффектный"],
    },
]


# -------------------- ОБРАБОТКА ФОТО --------------------

def prepare_image(uploaded_file):
    """Безопасно открывает и уменьшает изображение."""
    image = Image.open(uploaded_file)
    image = ImageOps.exif_transpose(image).convert("RGB")
    image.thumbnail((900, 900))
    return image


def extract_palette(image, count=5):
    """Извлекает основные цвета фотографии."""
    small = image.copy()
    small.thumbnail((180, 180))
    quantized = small.quantize(colors=count, method=Image.Quantize.MEDIANCUT)
    palette = quantized.getpalette()
    color_counts = quantized.getcolors()

    if not color_counts or not palette:
        return ["#D8B5B0"]

    color_counts.sort(reverse=True)
    result = []

    for amount, color_index in color_counts[:count]:
        i = color_index * 3
        rgb = tuple(palette[i:i + 3])
        hex_color = "#{:02X}{:02X}{:02X}".format(*rgb)
        result.append(hex_color)

    return result


def hex_to_rgb(color):
    color = color.lstrip("#")
    return tuple(int(color[i:i + 2], 16) for i in (0, 2, 4))


def color_distance(color_a, color_b):
    """Сходство оттенков с учетом восприятия RGB."""
    r1, g1, b1 = hex_to_rgb(color_a)
    r2, g2, b2 = hex_to_rgb(color_b)

    return sqrt(
        0.299 * (r1 - r2) ** 2
        + 0.587 * (g1 - g2) ** 2
        + 0.114 * (b1 - b2) ** 2
    )


def color_match(photo_palette, design_colors):
    if not photo_palette:
        return 50

    distances = [
        min(color_distance(p, d) for d in design_colors)
        for p in photo_palette
    ]

    # Не позволяем одному случайному пикселю
    # слишком сильно влиять на результат.
    best_distance = min(distances)
    return round(max(0, 100 * (1 - best_distance / 350)))


# -------------------- ПОДБОР ДИЗАЙНА --------------------

def recommend_designs(
    photo_palette,
    preferred_styles,
    occasion,
    length,
    shape,
    season,
    difficulty,
    price,
    preferred_color,
):
    results = []

    for design in DESIGNS:
        score = 0
        reasons = []

        # Совпадение палитры с фотографией
        match = color_match(photo_palette, design["colors"])
        score += match * 0.30

        if match >= 65:
            reasons.append("цвета близки к палитре фото")

        # Стиль
        if "Любой" in preferred_styles or design["style"] in preferred_styles:
            score += 20
            reasons.append("подходит по стилю")

        # Повод
        if occasion in design["occasion"]:
            score += 15
            reasons.append("подходит для выбранного повода")

        # Длина ногтей
        if length in design["length"]:
            score += 10
            reasons.append("подходит по длине")

        # Форма ногтей
        if shape in design["shapes"]:
            score += 10
            reasons.append("подходит по форме")

        # Сезон
        if season in design["season"]:
            score += 5

        # Сложность
        if difficulty == "Любая" or difficulty == design["difficulty"]:
            score += 5

        # Бюджет
        if price == "Любой" or price == design["price"]:
            score += 5

        # Дополнительный цветовой фильтр
        if preferred_color != "Не важно":
            if preferred_color == "Светлые" and design["style"] in [
                "Нюд", "Френч", "Минимализм", "Пастель", "Романтичный"
            ]:
                score += 10
            elif preferred_color == "Темные" and design["style"] in [
                "Классика", "Кошачий глаз"
            ]:
                score += 10
            elif preferred_color == "Яркие" and design["style"] in [
                "Классика", "Пастель", "Блестки"
            ]:
                score += 10

        results.append({
            **design,
            "score": round(score, 1),
            "photo_match": match,
            "reasons": reasons or ["подходит по общей палитре"],
        })

    results.sort(key=lambda item: item["score"], reverse=True)
    return results


# -------------------- СОСТОЯНИЕ ПРИЛОЖЕНИЯ --------------------

if "favorites" not in st.session_state:
    st.session_state.favorites = []

if "last_results" not in st.session_state:
    st.session_state.last_results = []


# -------------------- ИНТЕРФЕЙС --------------------

st.title("💅 AI Nail Stylist")
st.markdown(
    "### Твой персональный стилист маникюра\n"
    "Загрузи фотографию, настрой предпочтения и найди свой идеальный дизайн."
)

with st.sidebar:
    st.header("🎨 Настройки")

    preferred_styles = st.multiselect(
        "Любимые стили",
        [
            "Любой", "Нюд", "Френч", "Минимализм",
            "Романтичный", "Классика", "Хром",
            "Кошачий глаз", "Пастель", "Цветочный", "Блестки",
        ],
        default=["Любой"],
    )

    occasion = st.selectbox(
        "Для какого случая?",
        [
            "На каждый день", "Офис", "Свидание",
            "Вечеринка", "Свадьба", "Отпуск",
        ],
    )

    length = st.selectbox(
        "Длина ногтей",
        ["Короткие", "Средние", "Длинные"],
    )

    shape = st.selectbox(
        "Форма ногтей",
        ["Овал", "Миндаль", "Квадрат", "Мягкий квадрат"],
    )

    season = st.selectbox(
        "Сезон",
        ["Весна", "Лето", "Осень", "Зима"],
    )

    preferred_color = st.selectbox(
        "Цветовая гамма",
        ["Не важно", "Светлые", "Темные", "Яркие"],
    )

    difficulty = st.selectbox(
        "Сложность дизайна",
        ["Любая", "Легкий", "Средний", "Сложный"],
    )

    price = st.selectbox(
        "Стоимость в салоне",
        ["Любой", "Низкая", "Средняя", "Высокая"],
    )


tab1, tab2, tab3 = st.tabs([
    "✨ Подбор дизайна",
    "❤️ Избранное",
    "📚 Каталог",
])


# -------------------- ВКЛАДКА ПОДБОРА --------------------

with tab1:
    st.subheader("1. Загрузи фотографию")

    uploaded_file = st.file_uploader(
        "Фото рук, ногтей или понравившегося маникюра",
        type=["jpg", "jpeg", "png", "webp"],
        help="Выбери четкую фотографию с хорошим освещением.",
    )

    photo_palette = []

    if uploaded_file is not None:
        try:
            image = prepare_image(uploaded_file)

            col1, col2 = st.columns([1, 1])

            with col1:
                st.image(
                    image,
                    caption="Загруженная фотография",
                    use_container_width=True,
                )

            photo_palette = extract_palette(image)

            with col2:
                st.markdown("#### Палитра фотографии")
                st.caption(
                    "Основные цвета изображения, используемые "
                    "как источник цветового вдохновения."
                )

                swatches = st.columns(len(photo_palette))

                for i, color in enumerate(photo_palette):
                    with swatches[i]:
                        st.markdown(
                            f"""
                            <div style="
                                background:{color};
                                height:65px;
                                border-radius:12px;
                                border:1px solid #ddd;
                            "></div>
                            <p style="text-align:center;font-size:12px">
                                {color}
                            </p>
                            """,
                            unsafe_allow_html=True,
                        )

            st.info(
                "Важно: выделение цветов — это анализ всей фотографии, "
                "а не точное распознавание ногтей или оттенка кожи."
            )

        except Exception as error:
            st.error(f"Не удалось обработать изображение: {error}")
            photo_palette = []

    else:
        st.info(
            "Загрузи фото, чтобы использовать его палитру. "
            "Без фотографии подбор будет основан на твоих предпочтениях."
        )

    if not photo_palette:
        photo_palette = ["#E8B9AD", "#F4E6D8", "#D69B91"]

    st.divider()
    st.subheader("2. Получи персональные рекомендации")

    if st.button("✨ Подобрать маникюр", type="primary", use_container_width=True):
        results = recommend_designs(
            photo_palette=photo_palette,
            preferred_styles=preferred_styles or ["Любой"],
            occasion=occasion,
            length=length,
            shape=shape,
            season=season,
            difficulty=difficulty,
            price=price,
            preferred_color=preferred_color,
        )

        st.session_state.last_results = results

    results = st.session_state.last_results

    if results:
        st.subheader("Твои рекомендации")

        st.caption(
            "Процент — условный рейтинг соответствия выбранным "
            "параметрам и цветовой палитре, а не гарантия результата."
        )

        for design in results[:6]:
            with st.container(border=True):
                col1, col2 = st.columns([3, 1])

                with col1:
                    st.markdown(f"### {design['name']}")
                    st.write(design["description"])

                    st.write(
                        f"**Стиль:** {design['style']} · "
                        f"**Сложность:** {design['difficulty']}"
                    )

                    st.write(
                        f"**Длина:** {', '.join(design['length'])}"
                    )

                    st.caption(
                        "Почему рекомендовано: "
                        + "; ".join(design["reasons"])
                    )

                    color_cols = st.columns(len(design["colors"]))

                    for i, color in enumerate(design["colors"]):
                        with color_cols[i]:
                            st.markdown(
                                f"""
                                <div style="
                                    height:35px;
                                    background:{color};
                                    border-radius:8px;
                                    border:1px solid #ddd;
                                "></div>
                                <small>{color}</small>
                                """,
                                unsafe_allow_html=True,
                            )

                with col2:
                    st.metric("Рейтинг", f"{design['score']:.0f}/100")
                    st.caption(
                        f"Цветовое сходство: {design['photo_match']}%"
                    )

                    favorite_key = design["id"]
                    is_favorite = any(
                        item["id"] == favorite_key
                        for item in st.session_state.favorites
                    )

                    if st.button(
                        "❤️ В избранном" if is_favorite else "♡ Сохранить",
                        key=f"fav_{favorite_key}",
                    ):
                        if is_favorite:
                            st.session_state.favorites = [
                                item for item in st.session_state.favorites
                                if item["id"] != favorite_key
                            ]
                        else:
                            st.session_state.favorites.append(design)

                        st.rerun()

        export_rows = []

        for design in results:
            export_rows.append({
                "Название": design["name"],
                "Стиль": design["style"],
                "Рейтинг": design["score"],
                "Цветовое сходство": design["photo_match"],
                "Описание": design["description"],
                "Сложность": design["difficulty"],
                "Стоимость": design["price"],
                "Цвета": ", ".join(design["colors"]),
            })

        csv_data = pd.DataFrame(export_rows).to_csv(
            index=False
        ).encode("utf-8-sig")

        st.download_button(
            "📥 Скачать рекомендации в CSV",
            data=csv_data,
            file_name="nail_recommendations.csv",
            mime="text/csv",
            use_container_width=True,
        )


# -------------------- ВКЛАДКА ИЗБРАННОГО --------------------

with tab2:
    st.subheader("❤️ Избранные дизайны")

    favorites = st.session_state.favorites

    if not favorites:
        st.info(
            "Здесь будут дизайны, которые ты сохранишь "
            "на вкладке подбора."
        )
    else:
        for design in favorites:
            with st.container(border=True):
                st.markdown(f"### {design['name']}")
                st.write(design["description"])
                st.write(
                    f"**Цвета:** {', '.join(design['colors'])}"
                )

                if st.button(
                    "Удалить из избранного",
                    key=f"remove_{design['id']}",
                ):
                    st.session_state.favorites = [
                        item for item in st.session_state.favorites
                        if item["id"] != design["id"]
                    ]
                    st.rerun()

        favorites_df = pd.DataFrame([
            {
                "Название": item["name"],
                "Стиль": item["style"],
                "Описание": item["description"],
                "Цвета": ", ".join(item["colors"]),
            }
            for item in favorites
        ])

        st.download_button(
            "📥 Скачать избранное",
            data=favorites_df.to_csv(index=False).encode("utf-8-sig"),
            file_name="my_favorite_nails.csv",
            mime="text/csv",
        )


# -------------------- ВКЛАДКА КАТАЛОГА --------------------

with tab3:
    st.subheader("📚 Каталог дизайнов")

    search = st.text_input(
        "🔎 Найти дизайн по названию или описанию",
        placeholder="Например, нюд, розовый, блестки...",
    )

    all_styles = sorted(set(item["style"] for item in DESIGNS))

    catalog_style = st.selectbox(
        "Фильтр по стилю",
        ["Все стили"] + all_styles,
    )

    catalog = DESIGNS.copy()

    if catalog_style != "Все стили":
        catalog = [
            item for item in catalog
            if item["style"] == catalog_style
        ]

    if search.strip():
        query = search.strip().casefold()
        catalog = [
            item for item in catalog
            if query in item["name"].casefold()
            or query in item["description"].casefold()
            or query in item["style"].casefold()
            or any(query in tag.casefold() for tag in item["tags"])
        ]

    st.write(f"Найдено дизайнов: **{len(catalog)}**")

    for design in catalog:
        with st.container(border=True):
            st.markdown(f"### {design['name']}")
            st.write(design["description"])

            color_cols = st.columns(len(design["colors"]))

            for i, color in enumerate(design["colors"]):
                with color_cols[i]:
                    st.markdown(
                        f"""
                        <div style="
                            height:42px;
                            background:{color};
                            border-radius:8px;
                            border:1px solid #ddd;
                        "></div>
                        <small>{color}</small>
                        """,
                        unsafe_allow_html=True,
                    )

            st.caption(
                f"Стиль: {design['style']} · "
                f"Сложность: {design['difficulty']} · "
                f"Стоимость: {design['price']}"
            )

            already_saved = any(
                item["id"] == design["id"]
                for item in st.session_state.favorites
            )

            if st.button(
                "❤️ Сохранено" if already_saved else "♡ В избранное",
                key=f"catalog_fav_{design['id']}",
            ):
                if already_saved:
                    st.session_state.favorites = [
                        item for item in st.session_state.favorites
                        if item["id"] != design["id"]
                    ]
                else:
                    st.session_state.favorites.append(design)

                st.rerun()


# -------------------- ПОДВАЛ --------------------

st.divider()

st.markdown(
    "<p style='text-align:center;color:#9a8190'>"
    "💅 AI Nail Stylist · Найди свой стиль"
    "</p>",
    unsafe_allow_html=True,
)
