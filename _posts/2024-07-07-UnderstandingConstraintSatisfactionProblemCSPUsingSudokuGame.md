---
title: "스도쿠 게임을 통해 이해하는 제약 만족 문제CSP"
description: ""
coverImage: "/assets/img/2024-07-07-UnderstandingConstraintSatisfactionProblemCSPUsingSudokuGame_0.png"
date: 2024-07-07 01:59
ogImage:
  url: /assets/img/2024-07-07-UnderstandingConstraintSatisfactionProblemCSPUsingSudokuGame_0.png
tag: Tech
originalTitle: "Understanding Constraint Satisfaction Problem (CSP) Using Sudoku Game"
link: "https://medium.com/@canankorkut1/understanding-constraint-satisfaction-problem-csp-using-sudoku-game-b3b974946711"
---

## 자바스크립트를 사용한 단계별 수도쿠 및 CSP(Constraint Satisfaction Problem) 솔루션

Constraint Satisfaction Problem(CSP)은 특정 제약 조건 하에 변수의 적절한 값을 찾는 것을 목표로 합니다. CSP는 인공 지능과 최적화와 같은 많은 분야에서 중요한 역할을 합니다. 이 글에서는 수도쿠 게임을 통해 CSP를 설명하는 방법을 보여드릴 것입니다. 우리는 JavaScript로 수도쿠 게임을 코딩하면서 CSP의 기본 원리와 솔루션 방법을 단계별로 살펴볼 것입니다.

<img src="/assets/img/2024-07-07-UnderstandingConstraintSatisfactionProblemCSPUsingSudokuGame_0.png" />

## 수도쿠란 무엇인가요?

<div class="content-ad"></div>

수도쿠는 숫자들을 논리적으로 배치하는 퍼즐 게임입니다. 수도쿠 보드는 일반적으로 9x9 크기의 정사각형으로, 일부 셀에 숫자가 배치되어 있습니다. 플레이어의 목표는 각 행, 각 열 및 각 3x3 작은 정사각형 영역(지역이라고도 함)에 1부터 9까지의 숫자를 중복되지 않도록 배치하는 것입니다.

## CSP란?

특정 제약 조건 하에 변수의 적절한 값을 찾는 것을 목표로 합니다. 그 기본 구성 요소는 다음과 같습니다:

- 변수: 문제를 정의하는 기본 단위입니다. 각 변수는 특정 값을 가질 수 있습니다.
- 값 집합: 각 변수가 취할 수 있는 가능한 값들의 집합입니다. 이러한 값들은 문제 맥락에 따라 다를 수 있습니다.
- 제약 조건: 변수 간의 관계나 제한을 나타냅니다. 이러한 제약 조건은 특정 값의 조합을 방지하거나 특정 조건을 충족시키도록 보장합니다.

<div class="content-ad"></div>

## Sudoku를 CSP로 정의하기

- 변수: Sudoku 보드의 각 셀은 변수입니다. 보드의 81개 셀은 각각의 변수로 생각할 수 있습니다.
- 값 집합: 각 셀에 대한 가능한 값은 보통 1에서 9까지의 숫자입니다. 이러한 값은 해당 셀에 배치할 수 있는 가능한 숫자를 나타냅니다.
- 제약 조건: Sudoku 게임에서 각 행, 열 및 3x3 정사각 영역(영역이라고도 함)에 중복되는 숫자가 없어야 합니다. 이러한 제약 조건은 각 셀에 배치된 값이 게임의 규칙과 일치하도록 합니다.

## CSP 해결 방법

Sudoku를 해결하는 데 사용되는 일부 CSP 해결 방법은 다음과 같습니다:

<div class="content-ad"></div>

- 백트래킹: 이 방법에서는 각 단계에서 하나의 값이 셀에 할당되고 문제가 앞뒤로 진행되면서 해결됩니다.
- 포워드 체킹: 각 할당 후, 해당 변수의 값 집합에 대한 제약 조건을 확인하여 작동합니다. 이 방법을 통해 더 빠르고 효과적인 해결책을 제공할 수 있습니다.
- 제약 전파: 각 할당 후, 가능한 값 집합을 좁히는 과정으로 진행됩니다. 이 방법은 제약 조건이 더 강화되며 더 빠른 해결책 획득에 도움이 됩니다.

## 수도쿠 게임 코딩

여기에서는 JavaScript, HTML 및 CSS 언어를 사용했습니다. 이제 이 코드들을 살펴보겠습니다.

HTML:

<div class="content-ad"></div>

```json
---
태그 변경
---

그 HTML 구조에서 스도쿠 보드 및 다른 구성 요소는 특정 컨테이너 div로 구성되어 있습니다. 제목 및 스타일 파일은 head 태그에 정의되어 있으며, 스도쿠 보드는 main 태그 내의 game-container라는 컨테이너 div에 위치합니다. 이 컨테이너 내에는 board-container라는 div와 스도쿠 보드를 포함하는 board div가 있습니다. 셀은 JavaScript로 동적으로 작성되고 보드 div 내에 배치됩니다. info-container div는 추가 정보 및 오류 카운터 및 숫자 버튼과 같은 컨트롤용으로 사용됩니다.

CSS:

* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #f4f4f4;
    color: #333;
}

header {
    margin-top: 40px;
    font-size: large;
}

.game-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
    border-top: 1px solid #ccc;
    padding-top: 70px;
}

.board-container {
    margin-right: 50px;
}

#board {
    display: grid;
    grid-template-columns: repeat(9, 60px);
    grid-template-rows: repeat(9, 60px);
}

#board .cell {
    width: 60px;
    height: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid #ccc;
    font-size: 30px;
    background-color: #fff;
}

#board .cell:nth-child(3n) {
    border-right: 2px solid #333;
}

#board .cell:nth-child(3n+1) {
    border-left: 2px solid #333;
}

#board .cell:nth-child(n+19):nth-child(-n+27),
#board .cell:nth-child(n+46):nth-child(-n+54),
#board .cell:nth-child(n+73):nth-child(-n+81) {
    border-bottom: 2px solid #333;
}

#board .cell:nth-child(-n+9) {
    border-top: 2px solid #333;
}

.row-highlight {
    background-color: #b3b0b0  !important;
}

.col-highlight {
    background-color: #b3b0b0  !important;
}

.box-highlight {
    background-color: #b3b0b0 !important;
}
.selected {
    background-color: #919090 !important;
}

.cell.invalid {
    color: red;
}

#info-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.counters {
    margin-bottom: 20px;
}

.mistakes {
    display: flex;
    align-items: center;
    font-size: 27px;
}

.counter-label {
    margin-right: 5px;
}

.counter-value {
    color: red;
}

.buttons {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 9px;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
    margin-top: 100px;
}

.number-button {
    width: 100px;
    height: 100px;
    font-size: 30px;
    cursor: pointer;
}

#new-game-button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    width: 100%;
    height: 60px;
    margin-top: 15px;
}

<div class="content-ad"></div>

자바스크립트:

일반적으로 코드를 세 가지 주요 부분으로 검토할 수 있습니다:

- 게임 보드 및 셀 만들기: 먼저 게임 보드와 셀을 생성합니다.
- 사용자 상호 작용: 셀 선택, 숫자 버튼 클릭 및 새 게임 시작과 같은 사용자 상호 작용을 관리합니다.
- 수도쿠 퍼즐 해결: CSP 방법을 사용하여 수도쿠 퍼즐을 해결하고 새 퍼즐을 생성합니다.

사용된 CSP 방법:

<div class="content-ad"></div>

- Forward Checking:

- 'forwardChecking' function은 특정 셀에 숫자가 배치되었는지, 그리고 이 배치가 다른 셀의 제약 조건을 위반하는지 확인합니다.
- 이 함수는 동일한 숫자가 동일한 행, 열 및 3x3 상자에서 반복되지 않도록 확인합니다.
- 이 제약 조건이 위반되면 함수는 'false'를 반환하고, 그렇지 않으면 'true'를 반환합니다.

function forwardChecking(board, row, col, num) {
    for (let i = 0; i < 9; i++) {
        if (board[row][i] === num && i !== col) {
            return false;
        }
        if (board[i][col] === num && i !== row) {
            return false;
        }
    }
    const startRow = Math.floor(row / 3) * 3;
    const startCol = Math.floor(col / 3) * 3;
    for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
            if (board[startRow + i][startCol + j] === num && (startRow + i !== row || startCol + j !== col)) {
                return false;
            }
        }
    }
    return true;
}

2. Constraint Propagation:

<div class="content-ad"></div>

- constraintPropagation 함수는 셀에 숫자를 배치한 후 다른 셀의 도메인이 여전히 비어 있는지 확인합니다.
- 어떤 셀이라도 비어 있으면 함수는 ‘false’를 반환하고, 그렇지 않으면 ‘true’를 반환합니다.

function constraintPropagation(board, row, col, num) {
    for (let i = 0; i < 9; i++) {
        if (board[row][i] === 0 && i !== col) {
            const domain = getDomain(board, row, i);
            if (domain.length === 0) {
                return false;
            }
        }
        if (board[i][col] === 0 && i !== row) {
            const domain = getDomain(board, i, col);
            if (domain.length === 0) {
                return false;
            }
        }
    }
    const startRow = Math.floor(row / 3) * 3;
    const startCol = Math.floor(col / 3) * 3;
    for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
            if (board[startRow + i][startCol + j] === 0 && (startRow + i !== row || startCol + j !== col)) {
                const domain = getDomain(board, startRow + i, startCol + j);
                if (domain.length === 0) {
                    return false;
                }
            }
        }
    }
    return true;
}

3. Backtracking:

- ‘solve’ 함수는 백트래킹을 사용하여 스도쿠 퍼즐을 해결합니다.
- 빈 셀(값이 0인 셀)을 찾아 각 셀에 대해 가능한 값(도메인)을 시도합니다.
- 제약 조건을 위반하지 않는 값이 있는 경우 해당 값을 셀에 넣고 계속해서 해결을 진행합니다.
- 해결할 수 없으면 다시 돌아가 다른 값을 시도합니다.

<div class="content-ad"></div>

function solve(board) {
    const emptyCell = findEmptyCell(board);
    if (!emptyCell) {
        return true;
    }

    const [row, col] = emptyCell;
    const domain = getDomain(board, row, col);

    for (let num of domain) {
        if (isSafe(board, row, col, num)) {
            board[row][col] = num;
            if (forwardChecking(board, row, col) && constraintPropagation(board, row, col)) {
                if (solve(board)) {
                    return true;
                }
            }
            board[row][col] = 0;
        }
    }

    return false;
}

전체 JavaScript 코드:

document.addEventListener('DOMContentLoaded', (event) => {
    const board = document.getElementById('board');
    const numberButtons = document.querySelectorAll('.number-button');
    const newGameButton = document.getElementById('new-game-button');
    const mistakeCountElement = document.getElementById('mistake-count');
    let selectedCell = null;
    let mistakeCount = 0;

    // Sudoku 보드를 위해 81개의 셀 생성
    for (let i = 0; i < 81; i++) {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.setAttribute('data-index', i);
        cell.addEventListener('click', onCellClick);
        board.appendChild(cell);
    }

    // 셀 클릭 이벤트 처리
    function onCellClick(event) {
        const clickedCell = event.target;
        if (clickedCell.classList.contains('fixed')) {
            return; // 고정된 셀을 클릭하면 무시
        }

        clearHighlights();
        if (selectedCell) {
            selectedCell.classList.remove('selected');
        }
        selectedCell = event.target;
        selectedCell.classList.add('selected');
        highlightRelatedCells(selectedCell);
    }

    // 모든 셀 하이라이트 지우기
    function clearHighlights() {
        document.querySelectorAll('.cell').forEach(cell => {
            cell.classList.remove('selected', 'row-highlight', 'col-highlight', 'box-highlight');
        });
    }

    // 동일한 행, 열, 상자에 연관된 셀 하이라이트 표시
    function highlightRelatedCells(cell) {
        const index = parseInt(cell.getAttribute('data-index'));
        const row = Math.floor(index / 9);
        const col = index % 9;

        for (let i = 0; i < 9; i++) {
            board.children[row * 9 + i].classList.add('row-highlight');
            board.children[i * 9 + col].classList.add('col-highlight');
        }

        const startRow = Math.floor(row / 3) * 3;
        const startCol = Math.floor(col / 3) * 3;
        for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                board.children[(startRow + i) * 9 + startCol + j].classList.add('box-highlight');
            }
        }
    }

    // 숫자 버튼에 이벤트 리스너 추가
    numberButtons.forEach(button => {
        button.addEventListener('click', onNumberButtonClick);
    });

    // 셀에서 무효한 하이라이트 지우기
    function clearConflicts(cell) {
        const index = parseInt(cell.getAttribute('data-index'));
        const row = Math.floor(index / 9);
        const col = index % 9;

        for (let i = 0; i < 9; i++) {
            board.children[row * 9 + i].classList.remove('invalid');
        }

        for (let i = 0; i < 9; i++) {
            board.children[i * 9 + col].classList.remove('invalid');
        }

        const startRow = Math.floor(row / 3) * 3;
        const startCol = Math.floor(col / 3) * 3;
        for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                board.children[(startRow + i) * 9 + startCol + j].classList.remove('invalid');
            }
        }
    }

    // 3회 이상 실패 후 게임 입력 비활성화
    function disableGame() {
        cells.forEach(cell => {
            cell.removeEventListener('click', onCellClick);
        });

        numberButtons.forEach(button => {
            button.removeEventListener('click', onNumberButtonClick);
        });
    }

    // 숫자 버튼 클릭 이벤트 처리
    function onNumberButtonClick(event) {
        if (mistakeCount >= 3) {
            return; // 3회 실패 후 추가 입력 비활성화
        }

        if (selectedCell) {
            const number = event.target.textContent;
            clearConflicts(selectedCell);
            selectedCell.textContent = number;

            if (!isValidMove(selectedCell)) {
                mistakeCount++;
                mistakeCountElement.textContent = mistakeCount;
                selectedCell.classList.add('invalid');
                highlightConflicts(selectedCell);
                if (mistakeCount === 3) {
                    disableGame();
                    return;
                }
            } else {
                selectedCell.classList.remove('invalid');
            }
        }
    }

    // 간선 충돌 하이라이트
    function highlightConflicts(cell) {
        const index = parseInt(cell.getAttribute('data-index'));
        const row = Math.floor(index / 9);
        const col = index % 9;
        const number = cell.textContent;

        for (let i = 0; i < 9; i++) {
            const rowCell = board.children[row * 9 + i];
            if (rowCell !== cell && rowCell.textContent === number) {
                rowCell.classList.add('invalid');
            }
        }

        for (let i = 0; i < 9; i++) {
            const colCell = board.children[i * 9 + col];
            if (colCell !== cell && colCell.textContent === number) {
                colCell.classList.add('invalid');
            }
        }

        const startRow = Math.floor(row / 3) * 3;
        const startCol = Math.floor(col / 3) * 3;
        for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                const gridCell = board.children[(startRow + i) * 9 + startCol + j];
                if (gridCell !== cell && gridCell.textContent === number) {
                    gridCell.classList.add('invalid');
                }
            }
        }
    }

    // 새 게임 시작
    newGameButton.addEventListener('click', startNewGame);

    // 보드 재설정하고 새 퍼즐 생성
    function startNewGame() {
        const cells = document.querySelectorAll('.cell');
        cells.forEach(cell => {
            cell.textContent = '';
            cell.classList.remove('selected', 'row-highlight', 'col-highlight', 'box-highlight', 'invalid');
        });
        selectedCell = null;
        mistakeCount = 0;
        mistakeCountElement.textContent = mistakeCount;
        generatePuzzle();
    }

    // 이동이 올바른지 확인
    function isValidMove(cell) {
        const index = parseInt(cell.getAttribute('data-index'));
        const row = Math.floor(index / 9);
        const col = index % 9;
        const number = cell.textContent;

        for (let i = 0; i < 9; i++) {
            const cell = board.children[row * 9 + i];
            if (cell !== selectedCell && cell.textContent === number) {
                return false;
            }
        }

        for (let i = 0; i < 9; i++) {
            const cell = board.children[i * 9 + col];
            if (cell !== selectedCell && cell.textContent === number) {
                return false;
            }
        }

        const startRow = Math.floor(row / 3) * 3;
        const startCol = Math.floor(col / 3) * 3;
        for (let i = 0; i < 3; i++) {
            for (let j = 0; j < 3; j++) {
                const cell = board.children[(startRow + i) * 9 + startCol + j];
                if (cell !== selectedCell && cell.textContent === number) {
                    return false;
                }
            }
        }

        return true;
    }

    // 보드에서 빈 셀 찾기
    function findEmptyCell(board) {
        for (let row = 0; row < 9; row++) {
            for (let col = 0; col < 9; col++) {
                if (board[row][col] === 0) {
                    return [row, col];
                }
            }
        }
        return null;
    }

    // 셀에 대한 가능한 값을 가져오기
    function getDomain(board, row, col) {
        const domain = new Set(Array.from({ length: 9 }, (_, i) => i + 1));
        for (let i = 0; i < 9; i++) {
            domain.delete(board[row][i]);
            domain.delete(board[i][col]);
            domain.delete(board[Math.floor(row / 3) * 3 + Math.floor(i

<div class="content-ad"></div>

저는 Sudoku 예제를 활용하여 CSP의 기본 원칙과 해결 방법을 이해했어요. JavaScript로 Sudoku 게임을 코딩하여 CSP 방법을 실제로 보여드려보았죠. 이 글에서는 CSP의 기본 개념과 해결 방법을 배웠습니다.
```
