def get_input():
  # Gets sequences and scoring parameters from user
  seq1 = input("Enter the first sequence: ")
  seq2 = input("Enter the second sequence: ")

  match_score = int(input("Enter match score: "))
  mismatch_score = int(input("Enter mismatch score: "))
  gap_penalty = int(input("Enter gap penalty: "))

  return seq1, seq2, match_score, mismatch_score, gap_penalty

# Creates two matrices for separate scoring and tracing with placeholders
def initialize_matrix(rows, cols, gap_penalty):
  score_matrix = [[0 for _ in range(cols)] for _ in range(rows)]  # Integers for score
  trace_matrix = [['' for _ in range(cols)] for _ in range(rows)]  # Symbols to trace

  # Initializes the first row and column for both matrices
  for i in range(1, rows):
      score_matrix[i][0] = i * gap_penalty
      trace_matrix[i][0] = '↑'  # Gap in sequence 1
  for j in range(1, cols):
      score_matrix[0][j] = j * gap_penalty
      trace_matrix[0][j] = '←'  # Gap in sequence 2

  return score_matrix, trace_matrix

def fill_matrix(seq1, seq2, match_score, mismatch_score, gap_penalty):
  rows, cols = len(seq1) + 1, len(seq2) + 1
  score_matrix, trace_matrix = initialize_matrix(rows, cols, gap_penalty)

  # Fills in matrices
  for i in range(1, rows):
      for j in range(1, cols):
          match_mismatch_score = match_score if seq1[i-1] == seq2[j-1] else mismatch_score
          scores = [
              score_matrix[i-1][j-1] + match_mismatch_score,  
              score_matrix[i-1][j] + gap_penalty,            
              score_matrix[i][j-1] + gap_penalty             
          ]
          score_matrix[i][j] = max(scores)
          trace_matrix[i][j] = ''.join([
              '↖' if scores[0] == score_matrix[i][j] else '',
              '↑' if scores[1] == score_matrix[i][j] else '',
              '←' if scores[2] == score_matrix[i][j] else ''
          ])

  return score_matrix, trace_matrix

# Traces back through to find all optimal alignments
def traceback(seq1, seq2, score_matrix, trace_matrix):
  alignments = []
  def traceback_util(i, j, aligned_seq1, aligned_seq2):  # Helper function
      if i == 0 and j == 0:
          reversed_seq1 = ''.join([aligned_seq1[i] for i in range(len(aligned_seq1)-1, -1, -1)])
          reversed_seq2 = ''.join([aligned_seq2[i] for i in range(len(aligned_seq2)-1, -1, -1)])
          alignments.append((reversed_seq1, reversed_seq2))
          return
      if '↖' in trace_matrix[i][j]:  # Match/mismatch
          traceback_util(i-1, j-1, aligned_seq1 + seq1[i-1], aligned_seq2 + seq2[j-1])
      if '↑' in trace_matrix[i][j]:  # Gap in sequence 2
          traceback_util(i-1, j, aligned_seq1 + seq1[i-1], aligned_seq2 + '-')
      if '←' in trace_matrix[i][j]:  # Gap in sequence 1
          traceback_util(i, j-1, aligned_seq1 + '-', aligned_seq2 + seq2[j-1])

  traceback_util(len(seq1), len(seq2), "", "")  # Starts in the bottom right corner
  return alignments

def find_optimal_alignments(seq1, seq2, match_score, mismatch_score, gap_penalty):
  score_matrix, trace_matrix = fill_matrix(seq1, seq2, match_score, mismatch_score, gap_penalty)
  alignments = traceback(seq1, seq2, score_matrix, trace_matrix)
  return score_matrix, trace_matrix, alignments

def print_matrix(matrix):
  for row in matrix:
      print(' '.join([str(cell) for cell in row]))

def main():
  seq1, seq2, match_score, mismatch_score, gap_penalty = get_input()
  score_matrix, trace_matrix, alignments = find_optimal_alignments(seq1, seq2, match_score,   
  mismatch_score, gap_penalty)

  print("\nScore Matrix:")
  print_matrix(score_matrix)

  print("\nTrace-back Paths:")
  print_matrix(trace_matrix)

  print("\nOptimal Alignments:")
  alignment_number = 1
  for a1, a2 in alignments:  # Unpacks alignment tuple for a1 and a2
      print(f"Alignment {alignment_number}:\n{a1}\n{a2}")
      print(f"Score: {score_matrix[len(seq1)][len(seq2)]}")
      alignment_number += 1

if __name__ == "__main__":
  main()
