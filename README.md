import streamlit as st
def initialize_board():
    return [[" " for _ in range(3)] for _ in range(3)]
def check_winner(board):
    for row in board:
        if row[0] == row[1] == row[2] and row[0] != " ":
            return row[0]
    for col in range(3):
        if board[0][col] == board[1][col] == board[2][col] and board[0][col] != " ":
            return board[0][col]
    if board[0][0] == board[1][1] == board[2][2] and board[0][0] != " ":
        return board[0][0]
    if board[0][2] == board[1][1] == board[2][0] and board[0][2] != " ":
        return board[0][2]
    return None

def is_full(board):
    return all(cell != " " for row in board for cell in row)
def tic_tac_toe_ui():
    st.title("Tic-Tac-Toe Game")
    st.write("Click on the grid to make your move.")
    if "board" not in st.session_state:
        st.session_state.board = initialize_board()
    if "current_player" not in st.session_state:
        st.session_state.current_player = "X"
    if "winner" not in st.session_state:
        st.session_state.winner = None
    for i in range(3):
        cols = st.columns(3)
        for j in range(3):
            if st.session_state.board[i][j] == " " and st.session_state.winner is None:
                if cols[j].button(" ", key=f"{i}-{j}"):
                    st.session_state.board[i][j] = st.session_state.current_player
                    st.session_state.winner = check_winner(st.session_state.board)
                    if st.session_state.winner is None:
                        if is_full(st.session_state.board):
                            st.session_state.winner = "Tie"
                        else:
                            st.session_state.current_player = "O" if st.session_state.current_player == "X" else "X"
            else:
                cols[j].button(st.session_state.board[i][j], key=f"{i}-{j}", disabled=True)

    if st.session_state.winner:
        if st.session_state.winner == "Tie":
            st.success("It's a tie!")
        else:
            st.success(f"Player {st.session_state.winner} wins!")
    if st.button("Restart Game"):
        st.session_state.board = initialize_board()
        st.session_state.current_player = "X"
        st.session_state.winner = None

tic_tac_toe_ui()
