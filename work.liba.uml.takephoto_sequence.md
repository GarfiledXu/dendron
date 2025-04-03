---
id: fxpdgs3fi8mrls5xgfupuec
title: Takephoto_sequence
desc: ''
updated: 1742801126808
created: 1742354188037
---


@startuml

skinparam sequence {
ArrowColor DeepSkyBlue
ActorBorderColor DeepSkyBlue
LifeLineBorderColor blue
LifeLineBackgroundColor #A9DCDF

ParticipantBorderColor DeepSkyBlue
ParticipantBackgroundColor DodgerBlue
ParticipantFontName Impact
ParticipantFontSize 17
ParticipantFontColor #A9DCDF

ActorBackgroundColor aqua
ActorFontColor DeepSkyBlue
ActorFontSize 17
ActorFontName Aapex
}

title mediamodule

footer Author: 徐佳飞 | Date: 2025-03-24


participant main_process as T_M
participant takephoto_process as T_T1
participant takephoto_process_localserver as T_T2
participant takephoto_process_event_loop as T_T3
participant takephoto_process_takethread as T_T4


'主线程最终调用 mediamodule中的initConnects(){定义emit:MediaModule::initMediaService->映射CaptureWorker::init，触发MediaModule::initMediaService} 
'CaptureWorker::init(){定义emit:QTimer::timeout->映射CaptureWorker::checkUserdataPartSizeAndProcessIsIdle}
'
autonumber
T_M->T_M: new MediaModule{}
T_M->T_M: connect(&rk_pollevent::*, &MediaModule::*)
note left
connect(p_poll, &rk_pollevent::canUploadLogRdy, m_mediaModule, &MediaModule::uploadLogFile, Qt::QueuedConnection);
connect(p_node, &rk_nodethread::uploadPhotos, m_mediaModule, &MediaModule::uploadPhotoFiles);
connect(m_mediaModule, &MediaModule::endUploadedFingerDatas, p_poll, &rk_pollevent::ReportFignerOptReslut);
connect(m_mediaModule, &MediaModule::stopRecordForDiskIsLess, p_poll, &rk_pollevent::stopRecordForDiskIsLess);
connect(m_mediaModule, &MediaModule::endUploadVideoFile, [&] (QString file) {
    delVideoDTime = QDateTime::currentDateTime();
    p_poll->uploadVideoNums--;
    p_node->endUploadAndUpdateVideosList(file);
    qDebug("[MEDIA] End Upload One Video File! Get uploadVideoNums : %d", p_poll->uploadVideoNums);
});
connect(m_mediaModule, &MediaModule::waitForGetDocumentFileId, [this](QString topic_data) {
    p_poll->MqttPublish(topic_data, STAMPLE_INFO);
});
connect(m_mediaModule, &MediaModule::endUploadOnePhotoReply, p_node, &rk_nodethread::handleUploadedEndOnePhotoReply);
end note





@enduml