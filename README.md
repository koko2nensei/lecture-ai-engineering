# lecture-ai-engineering
AIエンジニアリング実践講座（公開用リポジトリ）


import streamlit as st
import pandas as pd
import numpy as np
import time

# ============================================
# ページ設定
# ============================================
st.set_page_config(
    page_title="Streamlit 基礎デモ",
    layout="wide",
    initial_sidebar_state="expanded"
)

# ============================================
# タイトルと概要
# ============================================
st.title(" Streamlit 基礎デモ")
st.caption("コメントを外しながら、Streamlitの機能を体験しましょう！")

# ============================================
# サイドバー
# ============================================
with st.sidebar:
    st.header(" デモのガイド")
    st.markdown("""
    - コメントを解除して機能を試そう！
    - UI変化をブラウザで確認
    """)
    st.divider()
    st.info("まずは左メニューから機能を選んでください")

# ============================================
# 画面本体
# ============================================
tab_main, tab_data, tab_chart = st.tabs(["UIデモ", "データ操作", "グラフ表示"])

with tab_main:
    st.header("基本的なUI要素")

    col1, col2 = st.columns(2)
    with col1:
        st.subheader("✏テキスト入力")
        name = st.text_input("あなたの名前", "ゲスト")
        st.success(f"こんにちは、{name}さん！")

        st.subheader("チェックボックス")
        if st.checkbox("チェックを入れるとメッセージが出ます"):
            st.info("チェックありがとうございます！")

        st.subheader("スライダー")
        age = st.slider("年齢を選んでください", 0, 100, 25)
        st.write(f"選択した年齢: {age}")

    with col2:
        st.subheader("ボタン")
        if st.button("ここをクリック！"):
            st.balloons()

        st.subheader("セレクトボックス")
        option = st.selectbox(
            "好きなプログラミング言語を選んでください",
            ["Python", "JavaScript", "Java", "C++", "Go", "Rust"]
        )
        st.write(f"選択した言語：{option}")

with tab_data:
    st.header("データの表示")
    
    df = pd.DataFrame({
        '名前': ['田中', '鈴木', '佐藤', '高橋', '伊藤'],
        '年齢': [25, 30, 22, 28, 33],
        '都市': ['東京', '大阪', '福岡', '札幌', '名古屋']
    })

    st.subheader("データフレーム表示")
    st.dataframe(df, use_container_width=True)

    st.subheader("テーブル表示")
    st.table(df)

    st.subheader("メトリクス表示")
    col1, col2, col3 = st.columns(3)
    col1.metric("温度", "23°C", "1.5°C")
    col2.metric("湿度", "45%", "-5%")
    col3.metric("気圧", "1013hPa", "0.1hPa")

with tab_chart:
    st.header("グラフ表示")

    st.subheader("ラインチャート")
    chart_data = pd.DataFrame(
        np.random.randn(20, 3),
        columns=['A', 'B', 'C'])
    st.line_chart(chart_data)

    st.subheader("バーチャート")
    bar_data = pd.DataFrame({
        'カテゴリ': ['A', 'B', 'C', 'D'],
        '値': [10, 25, 15, 30]
    }).set_index('カテゴリ')
    st.bar_chart(bar_data)

# ============================================
# ファイルアップロード機能
# ============================================
st.divider()
st.subheader("📁 ファイルアップロード機能")

uploaded_file = st.file_uploader("ファイルを選択してください", type=["csv", "txt"])
if uploaded_file is not None:
    bytes_data = uploaded_file.getvalue()
    st.write(f"アップロードされたファイルサイズ: {len(bytes_data)} bytes")
    if uploaded_file.name.endswith('.csv'):
        df = pd.read_csv(uploaded_file)
        st.write("CSVファイルのプレビュー:")
        st.dataframe(df.head())

# ============================================
# フッター
# ============================================
st.divider()
st.caption("Created with  by Matsuolab")
