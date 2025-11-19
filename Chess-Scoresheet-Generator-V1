from reportlab.lib.pagesizes import A5
from reportlab.pdfgen import canvas
import chess.pgn
from pdf2image import convert_from_path

def pgn_to_scoresheet_unlimited(pgn_path: str, output_pdf: str = "scoresheet.pdf", export_jpg: bool = True, poppler_path: str = None):
    """
    Erstellt ein mehrseitiges DIN A5 Scoresheet mit 3 Spalten pro Zeile.
    Optional: Export jeder Seite als JPG.
    """
    # PGN laden
    with open(pgn_path, "r", encoding="utf-8") as f:
        game = chess.pgn.read_game(f)

    c = canvas.Canvas(output_pdf, pagesize=A5)
    width, height = A5

    # Züge vorbereiten
    moves = []
    node = game
    while node.variations:
        node = node.variations[0]
        moves.append(node.san())

    # Layout-Konstanten
    x_positions = [40, 150, 260]
    lines_per_page = 20
    pairs_per_page = 3 * lines_per_page
    total_pairs = (len(moves) + 1) // 2
    move_number = 1
    i = 0
    page = 1

    while move_number <= total_pairs:
        # Kopfzeile
        c.setFont("Helvetica-Bold", 12)
        c.drawString(40, height - 40, f"Schach Scoresheet – Seite {page}")
        c.setFont("Helvetica", 9)
        if page == 1:
            c.drawString(40, height - 60, f"Event: {game.headers.get('Event', '')}")
            c.drawString(40, height - 75, f"Date: {game.headers.get('Date', '')}")
            c.drawString(40, height - 90, f"White: {game.headers.get('White', '')}")
            c.drawString(40, height - 105, f"Black: {game.headers.get('Black', '')}")
            # Elo-Werte (optional)
            white_elo = game.headers.get("WhiteElo")
            black_elo = game.headers.get("BlackElo")
            if white_elo:
                c.drawString(250, height - 90, f"Elo: {white_elo}")
            if black_elo:
                c.drawString(250, height - 105, f"Elo: {black_elo}")

        # Züge eintragen
        y = height - 130
        for line in range(lines_per_page):
            for col in range(3):
                if move_number > total_pairs:
                    break
                if i + 1 < len(moves):
                    white = moves[i]
                    black = moves[i + 1]
                    i += 2
                elif i < len(moves):
                    white = moves[i]
                    black = "____"
                    i += 1
                else:
                    white = "____"
                    black = "____"
                text = f"{move_number}. {white} {black}"
                c.drawString(x_positions[col], y, text)
                move_number += 1
            y -= 12
        c.showPage()
        page += 1

    c.save()

    # Optional: PDF → JPG
    if export_jpg:
        pages = convert_from_path(output_pdf, dpi=300, poppler_path=poppler_path)
        for i, page_img in enumerate(pages):
            page_img.save(f"{output_pdf.replace('.pdf', '')}_page_{i+1}.jpg", "JPEG")

    return output_pdf
