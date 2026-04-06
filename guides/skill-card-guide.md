# 스킬 카드 구성 가이드

## 개발 준비

스킬 카드를 구성하기 위해서는 `CardData`와 `ICard`는 반드시 상속해야 한다.

- `ICard` 이외에 다른 인터페이스를 사용해도 된다. (`IPieceCard`, `ITileCard`)
- `CardSO`를 만들어서 속성에 연결해야 한다.
  - 부가 설명 기능은 아직 추가되지 않았다. 추후 추가할 예정이다.

> 📷 예시 SO 이미지 → `assets/images/example-so.png` 로 교체 필요

---

## 기물/타일 스킬

- 반드시 `Selector` 속성을 사용해야 한다.
- `LoadPieceSelector` 사용 시 `selector.EnableSelector(this)`를 사용하여 선택자를 호출한다.

> 📷 예시 코드 이미지 → `assets/images/example-code.png` 로 교체 필요

---

## 전역 스킬

- 전역 스킬은 필수적인 필드나 속성은 없다.
- 필요한 경우, 속성을 따로 추가하여 사용할 수 있다.

---

## 예시 코드

### 기물 카드

```csharp
using System.Collections.Generic;
using UnityEngine;

public class TestSkill : CardData, IPieceCard
{
    private PieceSelector selector;

    private void Awake()
    {
        selector = FindFirstObjectByType<PieceSelector>();
    }

    public void LoadPieceSelector()
    {
        if (selector == null) selector = FindFirstObjectByType<PieceSelector>();
        selector.EnableSelector(this);
    }

    public void Execute(CardEffectArgs args = null)
    {
        Debug.Log($"카드 실행! : {DataSO.CardName}");
        List<Piece> pieces = args.Targets;
    }
}
```

### 타일 카드

```csharp
using System.Collections.Generic;
using UnityEngine;

public class TestTileSkill : CardData, ITileCard
{
    private TileSelector selector;

    private void Awake()
    {
        selector = FindFirstObjectByType<TileSelector>();
    }

    public void Execute(CardEffectArgs args = null)
    {
        Debug.Log($"카드 실행! : {DataSO.CardName}");
        List<Vector3Int> pieces = args.TargetPos;
    }

    public void LoadTileSelector()
    {
        if (selector == null) selector = FindFirstObjectByType<TileSelector>();
        selector.EnableSelector(this);
    }
}
```

### 전역 카드

```csharp
using UnityEngine;

public class GlobalSkill : CardData, ICard
{
    public void Execute(CardEffectArgs args = null)
    {
        Debug.Log($"카드 실행! : {DataSO.CardName}");
        Debug.Log($"제한 턴 여부: {DataSO.HasLimit}");
    }
}
```

---

## 사용 방법

### 기물/타일 카드

1. `Load타입Selector()` 함수 위에 `[ContextMenu("Execute")]` 추가
2. 카드 클래스가 적용된 오브젝트의 카드 컴포넌트 우클릭 → Execute 클릭
3. 기물 또는 타일 클릭 후 지정된 Selector 컴포넌트 우클릭 → Execute 클릭
4. 결과 확인

### 전역 카드

1. 아무 함수나 만들어서 `Execute()` 실행
2. 해당 함수 위에 `[ContextMenu("Execute")]` 추가
3. 카드 클래스가 적용된 오브젝트의 카드 컴포넌트 우클릭 → Execute 클릭
