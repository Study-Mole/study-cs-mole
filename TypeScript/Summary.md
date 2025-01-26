# TypeScript Summary

- [`type` vs `interface`](#type과-interface)
- [기본 타입과 타입 시스템](#기본-타입과-타입-시스템)

## `type` vs `interface`

`type`과 `interface`는 객체를 타이핑하기 위해 사용하는 키워드입니다. `interface`는 객체 타입만을 정의할 수 있는 반면, `type`은 모든 유형의 타입을 정의할 수 있습니다. 확장 시 `type`은 `&` 키워드를, `interface`는 `extends` 연산자를 사용하며 `interface`는 선언적 확장이 가능하다는 특징을 갖습니다.

## 기본 타입과 타입 시스템

기본적으로 TypeScript에는 `number`, `string`, `boolean`, `null / undefined`, `bigint`, `symbol`, `object`의 원시타입 7개가 있습니다. 이러한 원시타입 값은 서로 호환되지 않으며 프로그램의 기초적인 부품 역할을 합니다. 

TypeScript에서는 JavaScript에서 제시하지 않은 독자적인 타입 시스템을 제공합니다. 존재하는 모든 값을 오류없이 받아내는 `any`, 알 수 없는 타입을 나타내는 `unknown` 결코 발생하지 않는다는 의미인 `never` 등이 있습니다.

이외에도 여러 개의 타입을 결합할 수 있는 `Intersection`, 변수가 가질 수 있는 타입을 나열할 수 있는 `Union` 타입을 통해서 타입을 구조적으로 확장할 수도 있습니다.