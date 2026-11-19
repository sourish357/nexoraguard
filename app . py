import streamlit as st

# --- PAGE CONFIGURATION ---
st.set_page_config(page_title="Nexora Guard", page_icon="🛡️", layout="wide")
st.title("🛡️ Nexora Guard: AI Agent Security Checkpoint")

# --- INITIALIZE MEMORY (This keeps track of data while the app runs) ---
if 'logs' not in st.session_state:
    st.session_state.logs = [] # This is our tamper-evident record
if 'pending_approval' not in st.session_state:
    st.session_state.pending_approval = False

# --- CUSTOMER PERMISSIONS (The "Permission Slip") ---
st.sidebar.header("Customer Permission Slip")
max_amount = st.sidebar.number_input("Max Auto-Pay Amount (₹)", value=5000)
allowed_payee = st.sidebar.text_input("Authorized Payee", value="State Electricity Board")

# --- THE NEXORA GUARD CHECKPOINT (The Core Logic) ---
def nexora_guard_check(payee, amount):
    # 1. Check if the payee is allowed
    if payee != allowed_payee:
        return "BLOCKED", f"Security Alert: Unauthorized payee '{payee}'."
    
    # 2. Check if the amount is within the hard limit
    if amount > max_amount:
        return "BLOCKED", f"Security Alert: Amount ₹{amount} exceeds limit ₹{max_amount}."
    
    # 3. Check for "Borderline" amounts (Needs human approval)
    if amount > 4000: # Hardcoded borderline logic for demo
        return "PENDING", "Borderline amount. Requires customer approval."
    
    # 4. Otherwise, allow
    return "ALLOWED", "Transaction approved by Nexora Guard."

# --- THE UI ---
tab1, tab2 = st.tabs(["📱 Customer App", "🏦 Bank & Nexora Log"])

with tab1:
    st.header("Simulate an AI Agent Action")
    st.write("Click a button below to simulate the customer's AI agent trying to make a payment.")
    
    col1, col2 = st.columns(2)
    
    with col1:
        if st.button("⚠️ Simulate Phishing Attack (₹20,000 to Hacker)"):
            # The simulated AI output
            ai_payee = "Hacker John"
            ai_amount = 20000
            
            # Run it through Nexora Guard
            status, message = nexora_guard_check(ai_payee, ai_amount)
            
            # Save to logs
            st.session_state.logs.insert(0, {"Action": f"Pay {ai_payee} ₹{ai_amount}", "Status": status, "Message": message})
            
            if status == "BLOCKED":
                st.error(f"❌ {status}: {message}")

    with col2:
        if st.button("💡 Simulate Normal Bill (₹1,000 Electricity)"):
            ai_payee = "State Electricity Board"
            ai_amount = 1000
            
            status, message = nexora_guard_check(ai_payee, ai_amount)
            st.session_state.logs.insert(0, {"Action": f"Pay {ai_payee} ₹{ai_amount}", "Status": status, "Message": message})
            
            if status == "ALLOWED":
                st.success(f"✅ {status}: {message}")

    st.divider()
    
    # Borderline Scenario
    st.subheader("Borderline Transaction")
    if st.button("🤔 Simulate Borderline Bill (₹4,200 Electricity)"):
        ai_payee = "State Electricity Board"
        ai_amount = 4200
        
        status, message = nexora_guard_check(ai_payee, ai_amount)
        st.session_state.logs.insert(0, {"Action": f"Pay {ai_payee} ₹{ai_amount}", "Status": status, "Message": message})
        
        if status == "PENDING":
            st.warning(f"⏸️ {status}: {message}")
            st.session_state.pending_approval = True

    # Customer Approval Prompt
    if st.session_state.pending_approval:
        st.info("Your AI agent is waiting for your approval.")
        if st.button("👍 Approve ₹4,200 Payment"):
            st.session_state.pending_approval = False
            st.session_state.logs.insert(0, {"Action": f"Pay {allowed_payee} ₹4200", "Status": "ALLOWED BY USER", "Message": "User manually approved on phone."})
            st.success("✅ Payment Approved and Sent to Bank!")

with tab2:
    st.header("Tamper-Evident Activity Record")
    st.write("Every decision Nexora Guard makes is permanently logged here.")
    
    # Display the logs
    if len(st.session_state.logs) == 0:
        st.info("No activity yet. Go to the Customer App tab to simulate actions.")
    else:
        for log in st.session_state.logs:
            if log['Status'] == "BLOCKED":
                st.error(f"**{log['Action']}** -> {log['Status']}: {log['Message']}")
            elif log['Status'] == "PENDING":
                st.warning(f"**{log['Action']}** -> {log['Status']}: {log['Message']}")
            else:
                st.success(f"**{log['Action']}** -> {log['Status']}: {log['Message']}")
