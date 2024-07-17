---
title: "React Virtualized HOCs 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-06-ReactVirtualizedHOCs_0.png"
date: 2024-07-06 02:06
ogImage:
  url: /assets/img/2024-07-06-ReactVirtualizedHOCs_0.png
tag: Tech
originalTitle: "React Virtualized HOCs"
link: "https://medium.com/@onix_react/react-virtualized-hocs-c62364d25a50"
---

/assets/img/2024-07-06-ReactVirtualizedHOCs_0.png

하이어 오더 컴포넌트(HOCs)는 React Virtualized에서 기존 컴포넌트의 기본 로직을 수정하지 않고 기존 컴포넌트의 기능을 향상하고 확장하는 강력한 방법을 제공합니다. 이러한 HOCs는 다른 컴포넌트를 입력으로 사용하고 새로운 향상된 컴포넌트를 반환하여 복잡한 동작을 재사용하고 캡슐화할 수 있게 합니다. 이 기사에서는 React Virtualized의 몇 가지 주요 HOCs를 살펴보고, 그들이 어떻게 사용되어 더 많은 기능이 풍부하고 성능이 뛰어난 응용 프로그램을 구축하는 데 사용될 수 있는지를 보여줄 것입니다.

# React Virtualized의 몇 가지 주요 HOCs 개요:

## ArrowKeyStepper

<div class="content-ad"></div>

ArrowKeyStepper HOC을 사용하면 컴포넌트가 화살표 키 이벤트에 반응하여 키보드 탐색 지원을 갖춘 탐색할 수 있는 목록이나 그리드를 만드는 데 이상적입니다. 이는 접근성과 사용자 경험을 향상시키는 데 특히 유용합니다.

예시 사용법:

```js
ReactDOM.render(
  <ArrowKeyStepper columnCount={columnCount} rowCount={rowCount}>
    {({ onSectionRendered, scrollToColumn, scrollToRow }) => (
      <Grid
        columnCount={columnCount}
        onSectionRendered={onSectionRendered}
        rowCount={rowCount}
        scrollToColumn={scrollToColumn}
        scrollToRow={scrollToRow}
        {...otherGridProps}
      />
    )}
  </ArrowKeyStepper>,
  document.getElementById("example")
);
```

## AutoSizer

<div class="content-ad"></div>

AutoSizer HOC은 사용 가능한 공간에 따라 컴포넌트의 너비와 높이를 자동으로 조정하여 반응형 레이아웃을 보장합니다. 이는 동적으로 다른 화면 크기에 적응해야 하는 컴포넌트에 필수적입니다.

예시 사용법:

```js
ReactDOM.render(
  <AutoSizer>
    {({ height, width }) => (
      <List height={height} rowCount={list.length} rowHeight={20} rowRenderer={rowRenderer} width={width} />
    )}
  </AutoSizer>,
  document.getElementById("example")
);
```

## CellMeasurer

<div class="content-ad"></div>

CellMeasurer HOC은 사용자에게 보이지 않는 방식으로 셀의 내용을 일시적으로 렌더링하여 자동으로 셀의 내용을 측정합니다. 셀의 크기가 다를 수 있는 그리드에서는 정확한 측정을 통해 효율적인 렌더링을 할 수 있습니다.

예시 사용법:

```js
return (
    <CellMeasurer
      cache={cache}
      columnIndex={columnIndex}
      key={key}
      parent={parent}
      rowIndex={rowIndex}
    >
      <div
        style={
          ...style,
          height: 35,
          whiteSpace: 'nowrap'
        }
      >
        {content}
      </div>
    </CellMeasurer>
  );
```

## ColumnSizer

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경하세요.

<div class="content-ad"></div>

InfiniteLoader HOC은 사용자가 목록, 테이블 또는 그리드를 스크롤할 때 데이터를 가져오는 것을 관리합니다. 이는 무한 스크롤을 구현하여 사용자가 큰 데이터 세트를 스크롤하는 동안 추가 데이터를 로드하는 데 도움이 됩니다.

예시 사용법:

```js
ReactDOM.render(
  <InfiniteLoader isRowLoaded={isRowLoaded} loadMoreRows={loadMoreRows} rowCount={remoteRowCount}>
    {({ onRowsRendered, registerChild }) => (
      <List
        height={200}
        onRowsRendered={onRowsRendered}
        ref={registerChild}
        rowCount={remoteRowCount}
        rowHeight={20}
        rowRenderer={rowRenderer}
        width={300}
      />
    )}
  </InfiniteLoader>,
  document.getElementById("example")
);
```

## MultiGrid

<div class="content-ad"></div>

MultiGrid HOC는 그리드 컴포넌트에 고정된 열 및/또는 행을 추가하여 보다 복잡한 그리드 레이아웃을 생성하는 데 유용합니다. 이는 좀 더 나은 사용자 경험을 위해 고정 요소가 있는 그리드 레이아웃을 만드는 데 도움이 됩니다.

예제 사용법:

```js
function render() {
  return (
    <MultiGrid
      cellRenderer={cellRenderer}
      columnWidth={75}
      columnCount={50}
      fixedColumnCount={2}
      fixedRowCount={1}
      height={300}
      rowHeight={40}
      rowCount={100}
      width={width}
    />
  );
}
```

# ScrollSync

<div class="content-ad"></div>

ScrollSync HOC은 두 개 이상의 컴포넌트 간에 스크롤을 동기화하여 페이지의 다른 섹션 간에 연결된 스크롤 동작을 가능하게 합니다. 이는 사용자의 네비게이션을 향상시키고 더 일관된 사용자 경험을 만들어냅니다.

예제 사용법:

```js
function render(props) {
  return (
    <ScrollSync>
      {({ clientHeight, clientWidth, onScroll, scrollHeight, scrollLeft, scrollTop, scrollWidth }) => (
        <div className="Table">
          <div className="LeftColumn">
            <List scrollTop={scrollTop} {...props} />
          </div>
          <div className="RightColumn">
            <Grid onScroll={onScroll} {...props} />
          </div>
        </div>
      )}
    </ScrollSync>
  );
}
```

# WindowScroller

<div class="content-ad"></div>

WindowScroller HOC은 창의 스크롤 위치에 기반하여 테이블이나 목록 컴포넌트를 스크롤할 수 있도록 합니다. 이는 전체 페이지 스크롤과 동기화가 필요한 컴포넌트에 유용하며, 부드러운 스크롤 경험을 제공합니다.

예시 사용법:

```js
ReactDOM.render(
  <WindowScroller>
    {({ height, isScrolling, onChildScroll, scrollTop }) => (
      <List
        autoHeight
        height={height}
        isScrolling={isScrolling}
        onScroll={onChildScroll}
        rowCount={...}
        rowHeight={...}
        rowRenderer={...}
        scrollTop={scrollTop}
        width={...}
      />
    )}
  </WindowScroller>,
  document.getElementById('example')
);
```

React Virtualized 컴포넌트에 이러한 HOC을 통합함으로써, 개발자는 더 다양하고 성능이 우수한 응용 프로그램을 구축할 수 있습니다. 이러한 미리 구축된 특수 기능을 활용하여 핵심 컴포넌트의 기능을 강화하면서도 간단성과 재사용성을 희생하지 않습니다. 키보드 탐색, 동적 크기 조정, 무한 스크롤, 또는 동기화 스크롤링이 필요하더라도, React Virtualized HOC는 여러분의 요구 사항을 충족시키는 견고한 솔루션을 제공합니다.

<div class="content-ad"></div>

텔레그램 / 인스타그램 / 페이스북 / 쓰레드 / 깃허브
