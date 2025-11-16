# Personalized-Tutoring-for-Success
import streamlit as st
import pandas as pd

st.set_page_config(
    page_title="Sanders Tutoring For Success",
    layout="wide",
    initial_sidebar_state="collapsed",
)

CEO_EMAIL = "gandrassy@gmail.com"

if 'ceo_logged_in' not in st.session_state:
    st.session_state.ceo_logged_in = False
if 'visits' not in st.session_state:
    st.session_state.visits = {}
if 'anonymous_visits' not in st.session_state:
    st.session_state.anonymous_visits = 0

excluded_emails = ["gandrassy@gmail.com", "alexanderjsanders@gmail.com"]

if not st.session_state.ceo_logged_in:
    st.session_state.anonymous_visits += 1

def handle_ceo_login(email, password):
    if email == CEO_EMAIL and password == "SASHA_IS_THE_BEST_SON_EVER":
        st.session_state.ceo_logged_in = True
        st.experimental_rerun()
    else:
        st.error("Invalid email or password.")

def ceo_panel():
    st.title("CEO Management Panel")
    st.subheader(f"Welcome back, {CEO_EMAIL}!")

    st.markdown("---")
    st.header("Website Visit Counts")

    visit_list = [
        {"Email": email, "Visits": count}
        for email, count in st.session_state.visits.items()
    ]
    if st.session_state.anonymous_visits > 0:
        visit_list.append({"Email": "Anonymous Visitors", "Visits": st.session_state.anonymous_visits})

    if visit_list:
        visit_df = pd.DataFrame(visit_list)
        st.dataframe(visit_df, use_container_width=True)
    else:
        st.info("No tracked visits yet.")

    st.markdown("---")
    if st.button("Logout"):
        st.session_state.ceo_logged_in = False
        st.experimental_rerun()

if not st.session_state.ceo_logged_in:
    with st.sidebar:
        st.header("CEO Login")
        ceo_email_input = st.text_input("Email", value="", key="ceo_login_email")
        ceo_password_input = st.text_input("Password", type="password", key="ceo_login_password")
        if st.button("Login"):
            handle_ceo_login(ceo_email_input, ceo_password_input)
else:
    with st.sidebar:
        st.success("Logged in as CEO.")
        if st.button("Logout", key="sidebar_logout"):
            st.session_state.ceo_logged_in = False
            st.experimental_rerun()

if st.session_state.ceo_logged_in:
    ceo_panel()
else:
    st.title("Personalized Tutoring for Success")
    st.markdown("---")
    st.header("Welcome to Sanders Tutoring!")
    st.subheader("Expert help in Math, ELA, Social Studies, History, and Test Prep.")
    st.info("Ready to boost your grades? Explore our services below.")
    st.markdown("---")
    st.header("Why Choose Us?")
    col1, col2 = st.columns([1, 2])
    with col1:
        st.image("https://images.unsplash.com/photo-1550508544-b203a557b6f6", caption="Meet Your Expert Tutor")
    with col2:
        st.write("""
        We focus on individualized learning paths, helping students understand why they learn, not just what to memorize.
        With over 20 years of experience, we guarantee:
        - Proven Results: 95% of students improve their scores by a full letter grade.
        - Flexible Scheduling: Sessions designed around your life.
        - Subject Mastery: Deep expertise in Calculus, Physics, and SAT/ACT prep.
        """)
    st.markdown("---")
    st.header("Contact Us")
    st.write("For inquiries or more information, reach out at gandrassy@gmail.com or call 917 572 9194.")
    st.markdown("---")
    st.caption("© 2025 Sanders Tutoring")
