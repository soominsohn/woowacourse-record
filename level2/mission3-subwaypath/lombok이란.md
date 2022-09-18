# lombok이란?

[https://mangkyu.tistory.com/78](https://mangkyu.tistory.com/78)

[https://mangkyu.tistory.com/155?category=761302](https://mangkyu.tistory.com/155?category=761302)

lombok이란? 어노테이션 기반으로 코드를 자동완성 해주는 라이브러리

“코드 다이어트”

기계적으로 작성해야 하는 코드들이 상당히 많다.

### **[ Lombok의 장점 ]**

- 어노테이션 기반의 코드 자동 생성을 통한 생산성 향상
- 반복되는 코드 다이어트를 통한 가독성 및 유지보수성 향상
- Getter, Setter 외에 빌더 패턴이나 로그 생성 등 다양한 방면으로 활용 가능

lombok이 없다면 일어날 수 있는 일

```java
public class Store extends Common {

    private String companyName;                                 // 상호명
    private String industryTypeCode;                            // 업종코드
    private String businessCodeName;                            // 업태명
    private String industryName;                                // 업종명(종목명)
    private String telephone;                                   // 전화번호
    private String regionMoneyName;                             // 사용가능한 지역화폐 명
    private boolean isBmoneyPossible;                           // 지류형 지역화폐 사용가능 여부
    private boolean isCardPossible;                             // 카드형 지역화폐 사용가능 여부
    private boolean isMobilePossible;                           // 모바일형 지역화폐 사용가능 여부
    private String lotnoAddr;                                   // 소재지 지번주소
    private String roadAddr;                                    // 소재지 도로명주소
    private String zipCode;                                     // 우편번호
    private double longitude;                                   // 경도
    private double latitude;                                    // 위도
    private String sigunCode;                                   // 시군 코드
    private String sigunName;                                   // 시군 이름

    public String getCompanyName() {
        return companyName;
    }

    public void setCompanyName(String companyName) {
        this.companyName = companyName;
    }

    public String getIndustryTypeCode() {
        return industryTypeCode;
    }

    public void setIndustryTypeCode(String industryTypeCode) {
        this.industryTypeCode = industryTypeCode;
    }

    public String getBusinessCodeName() {
        return businessCodeName;
    }

    public void setBusinessCodeName(String businessCodeName) {
        this.businessCodeName = businessCodeName;
    }

    public String getIndustryName() {
        return industryName;
    }

    public void setIndustryName(String industryName) {
        this.industryName = industryName;
    }

    public String getTelephone() {
        return telephone;
    }

    public void setTelephone(String telephone) {
        this.telephone = telephone;
    }

    public String getRegionMoneyName() {
        return regionMoneyName;
    }

    public void setRegionMoneyName(String regionMoneyName) {
        this.regionMoneyName = regionMoneyName;
    }

    public boolean isBmoneyPossible() {
        return isBmoneyPossible;
    }

    public void setBmoneyPossible(boolean bmoneyPossible) {
        isBmoneyPossible = bmoneyPossible;
    }

    public boolean isCardPossible() {
        return isCardPossible;
    }

    public void setCardPossible(boolean cardPossible) {
        isCardPossible = cardPossible;
    }

    public boolean isMobilePossible() {
        return isMobilePossible;
    }

    public void setMobilePossible(boolean mobilePossible) {
        isMobilePossible = mobilePossible;
    }

    public String getLotnoAddr() {
        return lotnoAddr;
    }

    public void setLotnoAddr(String lotnoAddr) {
        this.lotnoAddr = lotnoAddr;
    }

    public String getRoadAddr() {
        return roadAddr;
    }

    public void setRoadAddr(String roadAddr) {
        this.roadAddr = roadAddr;
    }

    public String getZipCode() {
        return zipCode;
    }

    public void setZipCode(String zipCode) {
        this.zipCode = zipCode;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public String getSigunCode() {
        return sigunCode;
    }

    public void setSigunCode(String sigunCode) {
        this.sigunCode = sigunCode;
    }

    public String getSigunName() {
        return sigunName;
    }

    public void setSigunName(String sigunName) {
        this.sigunName = sigunName;
    }

}
```

이건 쫌..

Getter, Setter, Equals, ToString 등

반복적인 작업이 필요할까?

```java
@Getter 
@Setter 
public class Store extends Common { private String companyName;

*// 상호명*

private String industryTypeCode;

*// 업종코드*

private String businessCodeName;

*// 업태명*

private String industryName;

*// 업종명(종목명)*

private String telephone;

*// 전화번호*

private String regionMoneyName;

*// 사용가능한 지역화폐 명*

private boolean isBmoneyPossible;

*// 지류형 지역화폐 사용가능 여부*

private boolean isCardPossible;

*// 카드형 지역화폐 사용가능 여부*

private boolean isMobilePossible;

*// 모바일형 지역화폐 사용가능 여부*

private String lotnoAddr;

*// 소재지 지번주소*

private String roadAddr;

*// 소재지 도로명주소*

private String zipCode;

*// 우편번호*

private double longitude;

*// 경도*

private double latitude;

*// 위도*

private String sigunCode;

*// 시군 코드*

private String sigunName;

*// 시군 이름*

}
```

@AllArgsConstructor는 모든 변수를 사용하는 생성자를 자동완성 시켜주는 어노테이션

@NoArgsConstructor는 어떠한 변수도 사용하지 않는 기본 생성자를 자동완성 시켜주는 어노테이션

@RequiredArgsConstructor

@RequiredArgsConstructor는 특정 변수만을 활용하는 생성자를 자동완성 시켜주는 어노테이션. 

생성자의 인자로 추가할 변수에 @NonNull 어노테이션을 붙여서 해당 변수를 생성자의 인자로 추가할 수 있다. 

아니면 해당 변수를 final로 선언해도 의존성을 주입받을 수 있다.

@ToString 어노테이션을 활용하면 클래스의 변수들을 기반으로 ToString 메소드를 자동으로 완성시켜 준다.

@Data 어노테이션을 활용하면 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 자동완성 시켜준다. 실무에서는 너무 무겁고 객체의 안정성을 지키기 때문에 @Data의 활용을 지양한다.

[https://velog.io/@kay019/Lombok을-사용해야-할까](https://velog.io/@kay019/Lombok%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C)