# Usta-hub-z
Website for home maintenance services 
// pseudocode: on update of priceAgreements doc
exports.onPriceAgreementUpdate = functions.firestore
  .document('priceAgreements/{paId}')
  .onUpdate(async (snap, ctx) => {
    const after = snap.after.data();
    if (after.proposerAccepted && after.counterpartyAccepted && !after.bookingCreated) {
      // create booking with finalPrice
      const booking = { jobId: after.jobId, workerId: /*from job or chat*/, finalPrice: after.price, status: 'accepted', createdAt: admin.firestore.FieldValue.serverTimestamp() };
      const bRef = await admin.firestore().collection('bookings').add(booking);
      await snap.after.ref.update({ bookingCreated: true, bookingId: bRef.id });
      // notify both parties
    }
  });
