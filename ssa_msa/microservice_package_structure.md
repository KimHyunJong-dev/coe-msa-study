# microservice\_package\_structure

## boot

com.sdsmetro.metro.boot : MetroSpringBootAppliation.java

//com.sdsmetro.metro.war : for 다른 배포의 멤버로 참여

//com.sdsmetro.metro.client : \(client에서 접근\) client에서 Rest말고 adaptor 따로 제공

//com.sdsmetro.metro.adaptor.stub : \(옆 동네\)다른 서비스에서 접근 \(제한된 i/f에 특정 DTO만 접근하도록\) for loose coupling

//com.sdsmetro.metro.adaptor.skeleton

## service publishing

//com.sdsmetro.metro.sp.rest : MetroResource.java

## component platform \(spring security, weblogic...\)

//com.sdsmetro.metro.cp.jboss : MetroJBossLogic.java

//com.sdsmetro.metro.cp.spring : MetroSpringLogic.java

## domain \(POJO\)

com.sdsmetro.metro.domain.entity : Metro.java

com.sdsmetro.metro.domain.spec : interface MetroService.java

com.sdsmetro.metro.domain.spec.sdo : MetroCdo.java, MetroRdo.java

com.sdsmetro.metro.domain.logic : MetroLogic.java implements MetroService

com.sdsmetro.metro.domain.store : i/f

com.sdsmetro.metro.domain.store.memory : store impl

com.sdsmetro.metro.domain.proxy : interface MetroEventProducer.java \*ServiceAccessor.java \(외부서비스, Queue Access\)

## implementation \(resource access layer\)

com.sdsmetro.metro.ra.data.jpa

com.sdsmetro.metro.ra.data.sqlmap

com.sdsmetro.metro.ra.extservice\(service access\) : StationAdaptor implements ServiceAccessor

com.sdsmetro.metro.ra.event.kafka

com.sdsmetro.metro.ra.event.rabbitmq

