# Adobe Hackathon Round 1B: Persona-Driven Document Intelligence

## Project Overview

This project implements an intelligent document analysis system that extracts and ranks relevant sections from a collection of PDF documents based on a specific persona and their job-to-be-done requirements. The system processes 3-10 related PDF files and outputs a structured JSON with metadata, extracted sections, and subsection analysis.

## Features

- **PDF Processing**: Extracts text, structure, and formatting information from PDF documents
- **Section Detection**: Identifies headings and sections using font analysis and pattern matching
- **Semantic Relevance**: Uses sentence transformers to calculate relevance scores based on persona and job requirements
- **Ranked Output**: Provides top-ranked sections and subsections based on importance
- **Offline Operation**: Runs completely offline with no network dependencies
- **Fast Processing**: Designed to complete within 60 seconds for 3-5 documents

## Technical Approach

### Core Components

1. **DocumentProcessor Class**: Main orchestrator that handles PDF processing, section extraction, and relevance scoring
2. **PyMuPDF Integration**: Uses PyMuPDF for robust PDF text extraction with formatting information
3. **Sentence Transformers**: Employs the `all-MiniLM-L6-v2` model (~90MB) for semantic similarity calculations
4. **Heuristic Section Detection**: Combines font size, bold formatting, and pattern matching to identify headings

### Processing Pipeline

1. **PDF Extraction**: Extract text blocks with positioning and formatting metadata
2. **Section Identification**: Detect headings using font analysis and regex patterns
3. **Subsection Extraction**: Group content under identified sections
4. **Relevance Scoring**: Calculate semantic similarity between persona/job and content
5. **Ranking & Output**: Sort by relevance and generate structured JSON output

### Models and Libraries Used

- **PyMuPDF (fitz)**: PDF text extraction and structure analysis
- **Sentence Transformers**: Semantic similarity calculations using `all-MiniLM-L6-v2`
- **NumPy**: Numerical computations for similarity scoring
- **Regex**: Pattern matching for section detection

## Build and Run Instructions

### Prerequisites

- Docker installed on your system
- AMD64 architecture support

### Building the Docker Image

```bash
docker build --platform linux/amd64 -t mysolutionname:somerandomidentifier .
```

### Running the Solution

1. Create input and output directories:
```bash
mkdir -p input output
```

2. Place your PDF files in the `input` directory (3-10 related PDFs)

3. Run the container:
```bash
docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output --network none mysolutionname:somerandomidentifier
```

### Expected Output

The system will generate `output.json` in the `/app/output` directory containing:

- **Metadata**: Input documents, persona, job-to-be-done, timestamp
- **Extracted Sections**: Top 20 most relevant sections with rankings
- **Subsection Analysis**: Top 30 most relevant subsections with refined text

## Constraints Compliance

- ✅ **CPU Only**: No GPU dependencies
- ✅ **Model Size ≤ 1GB**: Uses ~90MB sentence transformer model
- ✅ **Processing Time ≤ 60s**: Optimized for 3-5 documents
- ✅ **Offline Operation**: No network access required
- ✅ **AMD64 Architecture**: Compatible with specified platform

## File Structure

```
.
├── main.py                 # Main processing script
├── requirements.txt        # Python dependencies
├── Dockerfile             # Container configuration
├── config.json            # Configuration for persona and job-to-be-done
├── test_solution.py       # Test suite for validation
├── run_tests.py           # Test runner script
├── README.md              # This file
└── approach_explanation.md # Detailed approach explanation
```

## Testing

### Running Tests

To validate the solution, run the test suite:

```bash
python run_tests.py
```

This will:
- Test constraint compliance (model size, offline operation)
- Validate document processing pipeline
- Generate sample output for verification

### Supported Document Types

The system is designed to work with various document types:
- Research papers
- Academic textbooks
- Financial reports
- Business documents
- Technical manuals

### Configuration

Edit `config.json` to test different personas and jobs-to-be-done:

```json
{
  "persona": "Research Analyst",
  "job_to_be_done": "Analyze key findings and methodologies from research documents"
}
```

## Performance Notes

- Processing time scales with document count and complexity
- Memory usage is optimized for the 16GB RAM constraint
- Model loading is done once at initialization for efficiency

## Troubleshooting

- Ensure PDF files are readable and not corrupted
- Check that input directory contains valid PDF files
- Verify Docker has sufficient resources allocated
- Monitor logs for any processing errors 