# Chess-Scoresheet-Generator-V1
🔍 What this code does:
- ✅ Automatic page breaks for long games
- ✅ Each page shows 60 pairs of moves (3 columns × 20 rows)
- ✅ Empty fields will be filled with ____
- ✅ Header only appears on page 1 (optionally expandable)

🧪 How to install:
    pip install python-chess reportlab pdf2image
  
💻 Use example: PGN → PDF + JPG

pgn_to_scoresheet_unlimited(
    pgn_path="partie.pgn",
    output_pdf="scoresheet_marius.pdf",
    export_jpg=True,
    poppler_path="C:/Tools/poppler/bin"  # only in windows
)

📜 License
  This tool is free to use and can be adapted for club, tournament, or personal use.
